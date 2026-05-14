# Email Infrastructure Decision — Cloudflare Worker (current) vs AgentMail.to vs AgenticMail

**Status:** Decision recorded. Re-evaluate at customer #10.
**Decided:** 2026-05-14
**Owner:** Karthik (re-eval) | Tariq (re-eval trigger)
**Cross-ref:** `strategy/boardrooms/2026-05-14_claim-agent-progress-review.md` §5

---

## §0. TL;DR

For **Forge customer 1 through ~10**, we stay on the **Cloudflare Email Routing Worker** (`etymolt-inbox-parser`). It's already deployed, working, free, and capability-equivalent to the SaaS alternatives for our current scale.

For **Forge customer #10+** (when per-customer Cloudflare zone provisioning becomes operationally painful), we re-evaluate **AgentMail.to** (SaaS) vs **AgenticMail** (self-hosted MIT).

For **any tier**: we never integrate an agent layer (Openclaw, Browser Use, Skyvern) that crosses the Bureau Model line — see `FORGE_BUREAU_MODEL.md`.

---

## §1. The three contenders

### Cloudflare Email Routing Worker (current — deployed)

- `etymolt-inbox-parser` Worker at Cloudflare
- 12 aliases + catch-all on etymolt.com → Worker
- KV namespace `etymolt-inbox` (id `f55b87…475a`) for record storage with 7d TTL
- Worker auto-clicks magic links, extracts OTPs via regex, POSTs to `/v3/forge/email-inbound` webhook
- `apps/cloudflare-workers/inbox-parser/worker.js`
- `apps/api/scripts/fetch_inbox.py` helper to pull codes via Cloudflare API

**Pricing:** $0 (Cloudflare Free tier covers 1,000 emails/day per zone, 100,000 KV reads/day, unlimited Workers requests in Free)

### AgentMail.to (SaaS — YC S2025)

- Hosted email-inbox-for-AI-agents primitive
- Custom-domain support with DKIM/SPF/DMARC
- Real-time webhooks + WebSockets for incoming events
- Per-inbox programmatic creation
- TypeScript + Python SDKs + MCP server
- IMAP/SMTP access
- **No SMS support**
- Y Combinator S2025, $6M raised March 2026

**Pricing:** Not transparent; likely free dev tier + paid scale.

### AgenticMail (open source, MIT — `github.com/agenticmail/agenticmail`)

- Self-hosted Stalwart mail server in Docker
- 62-tool MCP server (email send/search/reply, multi-agent RPC)
- **Google Voice integration** for SMS (NOTE: GV is VoIP — same filtering issue as Twilio)
- Active maintenance (v0.9.8 May 14, 2026; 197 commits, 119 stars)
- MIT licensed — we own the stack

**Pricing:** Free (self-hosted; operational cost = VM running Docker)

---

## §2. Capability comparison

| Capability | Cloudflare (current) | AgentMail.to | AgenticMail |
|---|---|---|---|
| Receive email at brand domain | ✓ | ✓ | ✓ |
| DKIM/SPF/DMARC support | ✓ | ✓ | ✓ |
| Parse + extract OTP codes | ✓ (regex in worker.js) | ✓ (SDK helpers) | ✓ |
| Auto-click magic links | ✓ (HTTP GET in worker) | ✓ (paired Openclaw flow) | ✓ |
| Real-time webhook on inbound | ✓ (POSTs to our API) | ✓ (native) | ✓ |
| WebSocket events | ✗ | ✓ | ✓ |
| Programmatic per-customer inbox creation | ⚠️ (Cloudflare zone per customer = manual) | ✓ (1 API call) | ✓ (config edit) |
| SMS receive | ✗ (separate: TextVerified) | ✗ | ⚠️ Google Voice (VoIP — filtered by IG/Twitter/etc.) |
| Multi-agent RPC | ✗ | ✗ | ✓ |
| MCP server integration | ✗ | ✓ | ✓ |
| Self-hosted (no vendor lock-in) | ⚠️ (Cloudflare lock-in) | ✗ | ✓ |
| Cost at 1 customer | $0 | $? | $20-30/mo VM |
| Cost at 100 customers | $0 (under Free tier) | $? × 100 | Same $20-30/mo VM |
| Operational complexity | Low (one Worker + KV) | Low (vendor managed) | Medium (Docker, Stalwart admin) |

---

## §3. Decision matrix

### Today (customer 0-1): Cloudflare Worker

**Rationale:**
- Already deployed, already working, already tested live.
- Capability-equivalent to alternatives for our current load.
- $0 cost.
- Switching now is sunk-cost waste.
- AgentMail.to and AgenticMail bring no new capabilities for our 1-customer use case.

### Mid-scale (customer 2-10): Stay on Cloudflare Worker

**Rationale:**
- Per-customer Cloudflare zone provisioning is annoying but tractable at this scale.
- Each customer needs their own brand domain (which they bring) — we configure Email Routing on it.
- Existing automation (`apps/api/services/claim_agent/cloudflare.py`) provisions zones programmatically.
- Cost remains $0.

### Scale-up trigger (customer 10+): Re-evaluate

**Forcing functions that trigger re-evaluation:**

1. **Operational pain:** Cloudflare zone setup taking >30 min per customer (we'd need to industrialize this).
2. **Per-customer feature divergence:** Each customer wants different alias routing, different webhook destinations, different storage — Worker code branches multiply.
3. **MCP-native customer ask:** Forge customers want their own MCP servers for their agents to talk to their Forge inbox — AgentMail's MCP server is ready-made; we'd build it ourselves on Cloudflare.
4. **WebSocket / real-time push** required for sub-second OTP delivery (current poll latency ~3s acceptable).

When 2 of 4 triggers fire, re-convene the board.

### Long-term (Forge mature product): AgenticMail (self-hosted)

**Why AgenticMail over AgentMail.to at scale:**
- MIT licensed → we own the data plane, no SaaS dependency
- Self-hosted → keeps customer email out of a vendor's database (better for legal/jurisdiction story)
- Multi-agent RPC → useful for cross-customer agent coordination if/when that emerges
- Single Docker container → operationally manageable for our team size

**Why NOT today:**
- Customer-1 use case doesn't need any of the above.
- AgenticMail SMS path uses Google Voice = VoIP = same filtering issue as Twilio. Doesn't replace TextVerified.
- Self-hosting trades vendor risk for ops risk; not worth it until we have enough customers to justify dedicated ops.

---

## §4. What we explicitly are NOT considering

Per `FORGE_BUREAU_MODEL.md`, we DO NOT integrate:

- **Openclaw** (AgenticMail's paired agent) — performs automated signups
- **Browser Use** — submits forms
- **Skyvern** — same
- Any other agent layer that crosses the "submit Create Account button" line

These are categorically off-limits regardless of how much they'd help operationally.

---

## §5. Cost of staying vs cost of switching

### Cost of staying on Cloudflare (1-10 customers)

- $0/mo infrastructure
- Founder time to provision zones: ~15 min per customer (mostly automated)
- Engineering time for per-customer feature requests: ~2 hours per customer requirement

### Cost of switching to AgentMail.to

- Migration eng time: ~3-5 days (rewrite worker.js logic in AgentMail SDK, port webhook handling, port KV reads, test, document)
- Recurring vendor fee: unknown but non-zero
- Trade Cloudflare lock-in for AgentMail.to lock-in (no net win)
- Need to migrate existing `etymolt-inbox-parser` Worker traffic (single zone, modest)

### Cost of switching to AgenticMail

- Migration eng time: ~7-10 days (provision VM, learn Stalwart, port worker, port helpers, document)
- Recurring infra fee: $20-30/mo (VM)
- Ops burden: keep Docker + Stalwart updated, handle outages
- Net win on data sovereignty + MCP-native integration

**At customer 1, neither switch's cost is justified.** The decision postpones cleanly.

---

## §6. Decision triggers (re-evaluate when…)

Convene the board to revisit this decision when ANY of:

- [ ] Customer count reaches 10
- [ ] A customer demands sub-second OTP delivery (we'd need WebSocket)
- [ ] A customer demands MCP-server access to their Forge inbox
- [ ] Cloudflare changes pricing on Email Routing or Workers Free tier
- [ ] We get an exfiltration / data-sovereignty audit ask (would push toward AgenticMail)
- [ ] Per-customer zone provisioning takes >1 hour of founder time per customer
- [ ] AgentMail.to publishes a pricing page indicating <$50/mo for our usage (would change math)

---

## §7. Cross-references

- **`strategy/boardrooms/2026-05-14_claim-agent-progress-review.md`** §5 — original decision
- **`docs/runbooks/FORGE_BUREAU_MODEL.md`** — what these tools can/can't do per legal posture
- **`apps/cloudflare-workers/inbox-parser/worker.js`** — current Worker source
- **`apps/api/scripts/fetch_inbox.py`** — KV-polling helper
- **`apps/api/services/claim_agent/cloudflare.py`** — programmatic zone + Email Routing setup
- **AgentMail.to docs:** https://www.agentmail.to/docs/api/v2
- **AgenticMail repo:** https://github.com/agenticmail/agenticmail

---

*Re-open this doc when a §6 trigger fires. Until then, the answer is "Cloudflare Worker, and we know why."*

# Etymolt Forge Lite — Customer Walkthrough v0

**Price:** $499 (one-time)
**Time on customer side:** ~20-30 active minutes spread across 24-48 hours
**Status:** v0 — drag-tested on etymolt.com (customer-zero) 2026-05-14
**Cross-ref:** `FORGE_BUREAU_MODEL.md` for what we do/don't do legally

---

## §0. What you get

After 48 hours, you walk away with:

| Asset | Where it lives | Effort to maintain |
|---|---|---|
| Your domain at Cloudflare with DNSSEC | Cloudflare dashboard | Zero — set and forget |
| `*@<yourdomain>.com` inbox that auto-clicks email verifications | Cloudflare Worker | Zero |
| Brand kit (logo, favicons, OG image, profile pic, twitter banner) | Your web repo | Re-generate anytime |
| `@yourbrand` claimed on 6 platforms: GitHub, Twitter, Discord, Reddit, Notion, Slack | The platforms | Standard account ownership |
| USPTO trademark filing draft (TEAS Plus, Class 9/35/42) | Markdown doc, ready to copy-paste | Self-submit ~30 min |
| (Pro +$200): ENS DNS-on-Chain binding + Farcaster fname + onchain resolver records | Ethereum mainnet + Base | Set and forget; ~$25-35 in gas paid by you |

---

## §1. Your effort budget

Total ~20-30 active minutes:

| Step | Active customer time | Wall-clock |
|---|---|---|
| Email confirmation (you say "yes I want this") | 30 sec | — |
| Domain handoff (you give us your registrar credentials OR you do the NS update yourself) | 5 min | — |
| 6× platform signups, ~2 min each (open form, paste password, solve CAPTCHA, submit) | 12 min | — |
| 6× OAuth approvals after signup | 1 min total | — |
| USPTO declaration sign + payment | 5 min | — |
| (Pro): Fund brand wallet + click "go" on ENS+Farcaster claim | 3 min | — |

Wall-clock 24-48 hours because DNSSEC propagation + DS-record-at-registrar + Railway deploy + ENS proveAndClaim mining all run in the background.

---

## §2. The flow, end to end

### Day 0 — kickoff (~10 min)

1. You sign up at `forge.etymolt.com/start` (or wherever the SKU lands).
2. Pay $499 (Pro: $699 with ENS+Farcaster).
3. Check the attestation: *"I confirm I will personally create each platform account, accept each platform's Terms of Service, and sign any legal filings myself."* (See `FORGE_BUREAU_MODEL.md` §5 for full text.)
4. Tell us:
   - Your brand name (we already cleared this via etymolt-the-engine — you wouldn't be here otherwise)
   - Your existing domain (if any) OR ask us to procure one
   - Your contact email (for our communications, not for the brand identity)
   - Your registrar credentials OR willingness to update NS records yourself

5. We assign you a Forge agent with a dedicated `claims-<yourbrand>@etymolt.com` working inbox.

### Day 0 — backend prep (you don't touch this; ~10 min)

Forge agent:
1. Creates Cloudflare zone for `<yourbrand>.com`
2. Mirrors any existing DNS records (we don't break your current setup)
3. Enables DNSSEC; emails you the DS record to paste at your TLD registrar (1-click step on your end)
4. Sets up Email Routing: 12 aliases (`hello@`, `support@`, `legal@`, etc.) + catch-all → our Worker
5. Deploys an inbox-parser Worker that auto-clicks any verification links it sees
6. Generates your brand kit via OpenAI's gpt-image-1
7. Generates strong unique passwords for the 6 platform accounts you'll create (vaulted; we surface to you in a secure-link)

### Day 0 — DS record at registrar (your 1-click step; 2 min)

We email you: *"Paste this DS record at your registrar. Click. Save."* Walkthrough screenshots included for GoDaddy / Namecheap / Cloudflare-Registrar / Google Domains.

DS propagation to .com TLD takes 10-60 minutes. We poll Cloudflare and email you when DNSSEC flips active.

### Days 0-2 — platform signups (your active time: ~15 min total)

For each of GitHub, Twitter, Discord, Reddit, Notion, Slack:

**Forge agent prep (1 min/platform):**
- Opens the signup URL in your browser (or guides you to open it in Incognito)
- Pre-fills email = `hello@<yourbrand>.com`, username = `<yourbrand>` (or first-available variant), display name
- For SMS-gated platforms (Twitter): pre-rents a non-VoIP US number from TextVerified, displays it for you to paste

**You (2 min/platform):**
- Type the generated password (we showed it to you; you type it — never us)
- Solve the platform's CAPTCHA/puzzle (~10 seconds)
- Click "Create account"

**Forge agent finish (1 min/platform):**
- Polls our inbox-parser Worker for the verification email (typically 5-15 sec to arrive)
- Auto-clicks magic links if present
- For 6-digit codes: surfaces the code to you in chat ("your GitHub code is 482917")
- Once your account is created and logged in, opens the Composio OAuth link → you click "Authorize"
- Composio stores the connected_account_id → Forge agent can now act on your behalf via official APIs

**Per-platform time:**
- GitHub: ~2 min (no SMS; just email verify)
- Notion: ~2 min (magic link, auto-clicked)
- Slack: ~3 min (creates workspace; you pick workspace URL)
- Discord: ~2 min (asks DOB; just pick one)
- Reddit: ~1.5 min (no email required at signup)
- Twitter/X: ~4 min (SMS verify via TextVerified, slight wait)

**Bluesky:** Not currently in TextVerified's catalog as of 2026-05-14; we've filed a support ticket requesting addition. When added, this becomes a 7th platform with ~3 min flow. Forge customers get pinged when ready.

### Day 1-2 — USPTO trademark prep (your active time: ~5 min)

Forge agent emails you a pre-filled USPTO TEAS Plus draft:
- All applicant fields filled (we asked for these at kickoff)
- Mark = `<yourbrand>` as standard character mark
- Three Nice classes (9, 35, 42) — appropriate for an AI/SaaS naming product; adjustable per industry
- 1(a) Use-in-commerce basis (provided your brand is shipping; otherwise 1(b) ITU)
- Specimen capture instructions per class
- The declaration text verbatim (you must sign yourself per USPTO rules)

**You (5 min):**
1. Open the draft, review
2. Open https://teas.uspto.gov/forms/tplus/
3. Copy-paste each field
4. Upload 3 specimens (we provide capture instructions)
5. E-sign declaration
6. Pay $750 ($250 × 3 classes; you pay directly to USPTO)

Total external cost: $750 to USPTO (not included in Forge fee — pass-through).

### (Pro only) Day 2-3 — onchain identity (your active time: ~3 min)

Forge agent creates a Privy-custodied TEE-isolated brand wallet for you. We surface the address.

**You:**
- Send ~0.01 ETH to the address on Ethereum mainnet (for ENS DNS-on-Chain proveAndClaim + resolver multicall)
- Send ~0.005 ETH on Base (for Farcaster IdGateway register + storage rent)
- Total ~$25-35 in gas at typical 2026 prices
- Tell us "wallet funded" in chat

**Forge agent (the cool part — automated):**
- Writes `_ens.<yourbrand>.com` TXT record (`a=<your_wallet_address>`)
- Fetches DNSSEC proof from api.ens.domains
- Submits `proveAndClaimWithResolver(...)` to ENS DNSRegistrar — your domain is now an ENS name
- Sets resolver records on Public Resolver via multicall:
  - addr → your wallet
  - url → `https://<yourbrand>.com`
  - description, com.twitter, com.github, email
  - avatar → `https://<yourbrand>.com/brand/<yourbrand>/profile.png`
- Calls Farcaster IdGateway.register() on Base with 1 storage unit
- Polls until your FID materializes (~30s)
- Submits EIP-712-signed claim of fname `<yourbrand>` to fnames.farcaster.xyz
- Sets profile metadata via Neynar API

**Outcome:** `<yourbrand>.com` resolves to your wallet in any ENS-aware app. `@<yourbrand>` on Farcaster posts from your brand wallet's identity. Both are permanent.

---

## §3. What's NOT in Forge Lite (and why)

| Excluded | Why |
|---|---|
| Account creation by Forge agent | Safety rule + platform ToS — see Bureau Model |
| Solving CAPTCHA | Same |
| Typing passwords for you | Same |
| TM filing on your behalf | UPL — you must sign the declaration personally |
| Madrid Protocol international TM | Pro+ add-on ($4K-$15K depending on jurisdictions, mostly USPTO + foreign filing fees) |
| Maintaining your handles | One-time setup; ongoing posting/management is your job (or you hire a social manager) |
| Office-action responses (TM) | Case-by-case attorney work; we recommend you keep one on retainer |
| Stripe / business banking setup | Requires legal entity formation; out of scope. We can refer to LLC-formation services |
| Cleanup of your existing personal account leakage | We respect what's already there — we don't touch your tattarw/personal accounts |

---

## §4. Day-zero customer-success checklist (for the Forge agent's runbook)

Before kickoff call:
- [ ] Run `etymolt verify --name <brand>` — confirms clearance verdict still holds
- [ ] Run `etymolt assess_taste --name <brand>` — sanity-check brand fit
- [ ] Provision Privy wallet (Pro tier)
- [ ] Pre-generate brand credentials vault

Kickoff call (15 min):
- [ ] Confirm attestation (Bureau Model)
- [ ] Collect domain status / registrar credentials
- [ ] Pick canonical signup email (default: `hello@<brand>.com`)
- [ ] Walk through what customer will see in the next 48h

Hour 1:
- [ ] Cloudflare zone created
- [ ] DNSSEC enabled, DS record emailed
- [ ] Email Routing aliases configured
- [ ] Inbox-parser Worker deployed
- [ ] Brand kit generated + committed to customer's repo (or zipped for handoff)

Hour 1-4:
- [ ] Customer confirms DS record pasted at registrar
- [ ] Poll until DNSSEC active at .com TLD
- [ ] Send "DNSSEC live" email

Hours 4-24:
- [ ] Customer signs up on 6 platforms (scheduled at their convenience)
- [ ] For each: Forge agent prepares → customer submits → agent fetches OTP → customer completes → agent OAuths

Hour 24:
- [ ] USPTO draft emailed; customer self-submits at their schedule

(Pro only) Hour 24-48:
- [ ] Customer funds wallet
- [ ] Forge agent runs `claim_brand.py --platforms web3_identity`
- [ ] Verify ENS lookup + Farcaster profile live

Final handoff:
- [ ] Send customer their credentials vault (passwords for all 6 accounts)
- [ ] Send customer a summary doc with all asset locations + login URLs
- [ ] Schedule 7-day check-in

---

## §5. What we learned from customer-zero (etymolt itself, 2026-05-14)

| Friction point | What we did | Productize? |
|---|---|---|
| Personal account logged in (tattarw on GitHub) | Incognito window | Document this prominently in §2 |
| Composio OAuth link 10-min TTL | Generate on-demand, just before customer clicks | Build "generate-just-in-time" wrapper, don't pre-batch |
| Brand kit Railway deploy lag | Push commit, wait 2-5 min | Trigger Railway redeploy via API at handoff |
| Bluesky not in TextVerified catalog | File support ticket | Maintain a "platform compatibility matrix" doc; warn customers if their target platform isn't bookable |
| Apex domain Railway binding | User does in Railway dashboard | Add to checklist or automate via Railway API |
| ENS DS-record paste at registrar | User does in registrar dashboard | Per-registrar walkthrough docs (GoDaddy / Namecheap / Cloudflare / Google) |
| Twitter requires SMS = needs TextVerified balance | Pre-rent number at handoff | Bake into checklist; surface "X verifications used / Y remaining" to customer |

---

## §6. Pricing tier ladder (boardroom-locked)

| Tier | Price | Includes |
|---|---|---|
| **Lite** | $499 | DNS + email + brand kit + 6-platform walkthrough + USPTO TM filing prep |
| **Pro** | $699 | Lite + ENS DNS-on-Chain + Farcaster fname + resolver records |
| **(future) Standard** | $999 | Lite + Watch service (USPTO bulk-delta monitoring, similarity alerts) + Office-action response template |
| **(future) Enterprise** | $1,999 | Standard + Pro + Madrid Protocol prep + named-attorney concierge |

Customer brings: ETH gas (~$25-35), TextVerified credits (~$5-10), USPTO filing fees ($750). All pass-through; not in our SKU.

---

## §7. Open follow-ups (post-customer-1)

- [ ] Build the `forge.etymolt.com/start` landing page + Stripe checkout
- [ ] Wire `attestation_v1` checkbox in onboarding flow (Karthik via Claude)
- [ ] Build the Forge agent "kickoff call" automation (calendar invite, intake form, brand-pre-check)
- [ ] Add per-registrar DS-record paste walkthroughs (GoDaddy is done; need Namecheap, Cloudflare Registrar, Google Domains)
- [ ] Build customer-credentials handoff (1Password sharing? HashiCorp Vault? plain encrypted PDF?)
- [ ] Watch service — wire daily USPTO bulk-delta monitoring against the customer's mark (`watch_etymolt_TM` cron)
- [ ] When customer #5: build the per-customer Cloudflare zone provisioner UI (today it's CLI)
- [ ] When customer #10: re-evaluate AgentMail.to vs AgenticMail per `EMAIL_INFRA_DECISION.md`

---

*v0 of this runbook is drag-tested on etymolt itself. Real customer #1 onboarding will surface friction we haven't seen yet — iterate this doc continuously.*

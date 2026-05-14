# The Bureau Model — Etymolt Forge's Legal Posture (and Marketing Asset)

**Status:** Canonical. Cite this doc in customer-facing copy, sales pages, ToS, and any conversation about what Forge does and doesn't do.
**Last updated:** 2026-05-14
**Boardroom decision:** `strategy/boardrooms/2026-05-14_claim-agent-progress-review.md` §5

---

## §0. TL;DR

> **Etymolt Forge is a form-filler and inbox aggregator. The customer creates each account themselves and accepts each platform's Terms of Service. We do not impersonate, scrape, or create accounts on the customer's behalf.**

That sentence is the entire legal posture. It's also a marketing asset: in a market where most "agent" tools quietly bypass ToS, we're the one that doesn't.

---

## §1. Why this posture exists

Three independent forcing functions converged on the same answer:

1. **Anthropic's safety rules.** Claude (the agent powering Forge) is hard-prohibited from creating accounts on behalf of users. This isn't tunable; it's an immutable rule baked into the model's safety layer. Any product built on Claude inherits this constraint.

2. **Platform Terms of Service.** GitHub, Twitter, Instagram, Reddit, Discord, Notion, Slack, Stripe — every major platform we touch has ToS language prohibiting:
   - Account creation by automated means
   - Bypassing CAPTCHA / bot-detection
   - Sharing or operating accounts on someone else's behalf without disclosure
   - Resale of access tokens

3. **Customer durability.** When a platform detects ToS-violating account creation, it bans the account. Permanently. A customer who paid for "@etymolt on Twitter" then loses that handle to a ban has lost the irreversible asset. Bureau Model accounts survive platform audits.

These three lines all draw the same boundary. We accepted it as a hard constraint and turned it into the positioning.

---

## §2. What Forge does — and doesn't — do

### ✅ What Forge does

| Action | Who performs | Why it's compliant |
|---|---|---|
| Provision a brand domain at Cloudflare | Forge agent | Customer owns the DNS account; we're a configured tool |
| Set up Email Routing + DNSSEC | Forge agent | Standard DNS administration |
| Generate brand assets (logo, favicons, OG image, profile pic) | Forge agent | Pure content creation |
| Receive verification emails at `*@<brand>.com` | Forge agent (via Cloudflare Worker) | Customer-owned domain, customer-authorized routing |
| Auto-click verification links in those emails | Forge agent | Verification is delegated; link is a one-time-use confirmation |
| Extract OTP codes from emails / SMS | Forge agent | The customer's own inbox |
| Rent non-VoIP SMS numbers via TextVerified | Forge agent on behalf of customer | Customer's TextVerified account, customer's credits |
| Pre-fill non-credential fields (email, username, display name) on signup forms | Forge agent | Standard form-assistance, no ToS bypass |
| Open the signup page, signal "next step" | Forge agent | Navigation only |
| Surface USPTO TEAS Plus draft values | Forge agent | Form-fill assistance for the customer to copy-paste |
| Set up custom-domain bindings (Bluesky `<brand>.com`, GitHub Pages) | Forge agent | Customer-authorized DNS edits on customer-owned domain |
| Bind ENS DNS-on-Chain after wallet funding | Forge agent | Customer-signed onchain transaction |
| Drive OAuth flows from the customer's already-created account | Forge agent | Standard OAuth — the customer authorizes the connection |

### ❌ What Forge does NOT do

| Action | Why not |
|---|---|
| Click "Create Account" on any platform | Safety rule + platform ToS |
| Type the customer's chosen password into a form | Safety rule (credential handling) |
| Solve CAPTCHA or human-verification puzzles | Platform ToS (anti-bot) |
| Operate accounts in the customer's name without disclosure | Identity-misrepresentation risk |
| Sign legal agreements (Stripe terms, USPTO declaration, ToS acceptance) on the customer's behalf | UPL/legal-attribution risk |
| Use scrapers that violate site ToS to harvest competitor / handle data | Platform ToS + civil liability (Meta v. Bright Data line of cases) |
| Use VoIP-based SMS to circumvent platform anti-bot filtering | Platform ToS spirit, even when not explicit letter |

If a feature would require any of the above, **we decline the feature**. We do not engineer around the rule.

---

## §3. The customer experience

**Customer pays for Forge Lite ($499) → 24-48 hours later they have:**

1. A registered domain (their pre-existing or Forge-procured) with DNS + DNSSEC + Email Routing + DKIM + SPF + DMARC fully configured.
2. A live `*@brand.com` inbox that auto-clicks verification links.
3. A generated brand kit (logo, favicons, OG image, profile pic, social banner) in their repo.
4. A walkthrough document for claiming the brand on 6 platforms (GitHub, Twitter, Discord, Reddit, Notion, Slack). Each platform takes ~2 minutes of the customer's active time.
5. A USPTO TEAS Plus filing draft, pre-filled with their values, ready for their signature.
6. (Pro tier +$200): ENS DNS-on-Chain binding + Farcaster fname registration + onchain resolver records.

**Active customer time required:** ~20 minutes across the 48-hour window. Most of that time is "click this URL, type a password, click submit."

**Active customer time WITHOUT Forge:** Empirically 1-2 days. Documented from customer interviews (Diego, May 2026: "I lost two days to this" — n=2).

**The pitch:** "Forge does 95% of the work. You do the 5% that matters legally."

---

## §4. Marketing copy implications

### What we say

> *"We're a courier, not a forger. We do every step we legally can — and we're explicit about the 5% only you can do."*

> *"Most agent tools quietly bypass platform ToS. We don't. That's why your handle is still there in six months."*

> *"Etymolt Forge: the only post-name automation that respects every platform's terms of service."*

> *"Sign your own account. We do the rest."*

### What we DON'T say

- ❌ "Fully automated brand identity setup" (false — customer does account creation)
- ❌ "Agent-driven signup" (misleading — the agent doesn't sign up)
- ❌ "We claim handles for you" (false — customer claims; we orchestrate)
- ❌ Anything implying we operate accounts on the customer's behalf
- ❌ Anything implying we sign legal agreements for the customer (USPTO, ToS)
- ❌ Anything implying the agent owns the resulting accounts

### Disclaimer (footer-grade, every page)

> *Etymolt Forge is a workflow and form-completion tool. The customer creates each platform account personally and is the sole owner of each account created. Etymolt does not act as an agent of record for any legal filing, including USPTO TEAS Plus applications. Customers are advised to consult a licensed attorney for trademark prosecution decisions.*

---

## §5. ToS attestation (in-product)

Before customer starts the Forge flow, they check this box:

> ☐ **I confirm that I will personally create each platform account, accept each platform's Terms of Service, and sign any legal filings (including USPTO trademark applications) myself. I understand that Etymolt Forge is a workflow tool, not an agent acting on my behalf.**

Storage: `customers.<id>.attestations.bureau_model_v1` with timestamp + IP.

Why: protects us legally if a platform alleges automated account creation, AND makes the customer's role explicit before they're committed.

---

## §6. Comparison to other agent stacks

| Tool / approach | Crosses Bureau Model line? | Risk profile |
|---|---|---|
| **Etymolt Forge** | No | Clean — explicit about its role |
| Browser Use (open-source) | Yes (can submit signup forms) | Customer faces ToS-violation risk |
| Skyvern (commercial) | Yes (browser automation including form submit) | Same |
| Openclaw (AgentMail-paired) | Yes (claims to do agent-driven signups) | Same |
| Magic Loops, Lindy, etc. | Depends on workflow | Customer accepts the risk per workflow |
| Manual VA / freelancer | No (human does signup) | Clean, but expensive ($30+/hour) |

We position against the agent stacks that cross the line: "they'll get your handle in 30 minutes, we'll get your handle and keep it."

---

## §7. Engineering implications

What this posture forbids us from doing in code:

- ❌ Headless browser automation that fills password fields or clicks "Sign up"
- ❌ Headless solving of CAPTCHA via solving services (2Captcha, AntiCaptcha, etc.)
- ❌ Calling unofficial / reverse-engineered platform signup APIs
- ❌ Integrating any third-party tool that performs the above (AgenticMail's Openclaw flow, Browser Use, Skyvern, etc.)

What this posture **does** allow:

- ✅ Headless browser navigation to open signup pages for the customer
- ✅ Pre-filling email + username + display name (non-credential, non-ToS-acceptance fields)
- ✅ Polling the customer's inbox for verification codes
- ✅ Auto-clicking magic links sent to the customer's inbox
- ✅ Driving OAuth flows from an already-created account
- ✅ Calling official platform APIs once authenticated

The line: we touch the customer's authenticated session, never the platform's account-creation funnel.

---

## §8. When this might change (and how)

This posture is durable but not eternal. Three scenarios would trigger a re-evaluation:

1. **A platform offers an official "agent-onboarding API"** with explicit ToS allowance for automated account creation. (Probable: 2027-2028 timeframe; some platforms may launch developer-facing brand-account-provisioning APIs.) → Forge would integrate that API, retaining attribution to the customer.

2. **Anthropic adds a "verified business-agent" tier** with platform-by-platform allowlists. → Forge would gate the relevant signup flows behind explicit business-verification of the customer.

3. **A customer pays for a "we're the agent of record" tier** (think: managed-services arm, not the SaaS product). → That's a different SKU with different legal structure. NOT Forge.

Any of these triggers a re-convening of the board to update this document.

---

## §9. Cross-references

- **`strategy/boardrooms/2026-05-14_claim-agent-progress-review.md`** — board decision establishing this posture
- **`strategy/boardrooms/2026-05-14_agent-vs-tool-and-post-name-automation.md`** — earlier board decision establishing form-filer + courier architecture
- **`docs/runbooks/USPTO_TM_FILING_DRAFT.md`** — applied: customer signs USPTO declaration; we prep
- **`docs/runbooks/FORGE_LITE_v0.md`** — applied: customer signup walkthrough with explicit "you click here" steps
- **`docs/runbooks/EMAIL_INFRA_DECISION.md`** — rationale for declining AgentMail/AgenticMail integrations that cross this line

---

*This document is canon. Update on board approval only.*

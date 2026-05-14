# Etymolt Forge — Terms of Service

**Effective:** 2026-05-15
**Version:** v1
**Authority:** Etymolt (sole proprietor: Tariq A, transitioning to LLC post-launch)
**Contact:** legal@etymolt.com (Cloudflare Email Routing → operator)

These Terms of Service ("Terms") govern your purchase and use of **Etymolt Forge**, a post-name automation product offered at `etymolt.com/forge`. By purchasing, you agree to these Terms.

---

## §1. What Forge is

Forge is a **workflow tool**. It performs the technical steps needed to launch a brand identity — DNS, email, brand assets, social handle preparation, USPTO trademark filing draft — and does NOT act as an agent of record for any account, filing, or contract.

The full operational specification is published at:
- [`FORGE_LITE_v0.md`](./FORGE_LITE_v0.md) — customer walkthrough
- [`FORGE_BUREAU_MODEL.md`](./FORGE_BUREAU_MODEL.md) — legal posture

These two documents are incorporated into these Terms by reference.

---

## §2. The Bureau Model

You acknowledge and agree to the **Bureau Model** legal posture, summarized:

> Etymolt Forge is a form-filler and inbox aggregator. **The customer creates each account themselves and accepts each platform's Terms of Service.** We do not impersonate, scrape, or create accounts on the customer's behalf.

Specifically, you agree that for every platform Forge prepares (GitHub, Twitter/X, Notion, Slack, Discord, Reddit, others as added):

- **You** click "Create Account" / "Sign up"
- **You** type the password (which Forge generates and shows to you)
- **You** solve any CAPTCHA, hCaptcha, or human-verification puzzle
- **You** accept that platform's Terms of Service
- **You** are the sole legal owner of the account created

Forge provides preparation: open the form, pre-fill non-credential fields, pre-rent a non-VoIP SMS number where required, watch for the verification code in your inbox/SMS, drive any subsequent OAuth back into Forge's automation layer.

This division is structural, not preferential. It is required by:
1. The safety rules of the underlying AI agent (Anthropic Claude) that drives Forge
2. The Terms of Service of every major social platform
3. Your protection from a platform ban that would destroy the brand identity you paid for

---

## §3. The 5% you must do yourself

You must personally perform each of the following:

- **Account creation** on each platform
- **Password typing** (we generate, you type)
- **CAPTCHA / hCaptcha / image puzzle** solving
- **ToS acceptance** at each platform
- **Verification of phone numbers** issued via TextVerified (you click "Send code" / paste the code we surface)
- **Signing the USPTO trademark declaration** (USPTO does not accept agent-signed declarations on TEAS Plus filings; only the applicant or a registered attorney can sign)
- **Paying USPTO filing fees** ($750 for three classes; we do not handle USPTO payment)
- **Funding the brand wallet** (Pro tier only; ~0.015 ETH on Ethereum mainnet + Base) — actually, on the Pro tier, we front the gas and reimburse from your payment; you pay $699 inclusive

The active time required of you is approximately **30 minutes**, distributed across the 48-hour Forge delivery window.

---

## §4. Payment terms

- **Forge Lite:** $499 USD, one-time, paid via Stripe Payment Link at checkout
- **Forge Pro:** $699 USD, one-time, paid via Stripe Payment Link at checkout
- **Pass-through costs (NOT included in Forge fee):**
  - USPTO TEAS Plus filing fee: $750 (3 classes × $250) — you pay USPTO directly
  - Pro tier ETH gas (~$25–35): we front it; included in your $699
  - TextVerified SMS rentals (~$0.25 per SMS-gated platform): included in your $499
- **Currency:** USD only
- **Sales tax / VAT:** Where applicable, calculated and added at checkout via Stripe Tax

Forge is sold as a one-time delivery. There is no subscription, no recurring fee, no auto-renewal.

---

## §5. Delivery commitment

Forge commits to delivering all infrastructure and brand-kit components within **48 hours of payment**. The 48-hour clock starts when:
1. Payment is successfully processed by Stripe, AND
2. You complete the kickoff intake form (we email you a link within 1 hour of purchase)

**Pause conditions** (clock pauses, then resumes when condition is met):
- You haven't pasted the DS record at your domain registrar (we can't proceed with DNSSEC until you do)
- You haven't completed a platform signup we've prepped (we can't OAuth into an account that doesn't exist)
- You haven't responded to a clarification email within 12 hours

**If we miss 48 hours not due to a pause condition,** we refund $99 per missed-12-hour bucket, capped at the full $499.

---

## §6. Refund policy

See [`REFUND_POLICY.md`](./REFUND_POLICY.md). Summarized:

- **Full refund** within 24 hours of purchase, before any DNS change is made
- **Partial refund** ($499 - $99) after DNS is configured but before platform signups complete
- **No refund** after platform signups complete (you have working accounts; we delivered)
- **USPTO $750 fee** is pass-through and refundable only per USPTO's own policy
- **Platform-ban remedy:** if a platform bans your handle within 30 days of claim despite our Bureau-Model compliance, we re-attempt with a variant handle at no charge; if we can't, 50% refund

---

## §7. What we don't do (and what happens if you ask)

We decline, automatically and without exception, to:

- Create accounts in your name
- Solve CAPTCHAs
- Type your password
- Sign legal agreements (Forge does not sign your USPTO declaration)
- Operate accounts in your name without disclosure
- Use scrapers that violate site Terms of Service
- Use VoIP/virtual SMS numbers that violate platform anti-bot policies
- Provide legal advice on trademark prosecution outcomes
- Promise specific examiner outcomes at the USPTO

If your purchase requires any of the above, **we refund in full and end the engagement.** No partial fulfillment is offered for such requests.

---

## §8. Customer responsibilities

You agree to:

- Provide accurate information at intake (legal name for USPTO, valid contact email, current registrar credentials if you're handing off NS changes)
- Personally create each platform account (see §3)
- Accept each platform's ToS at signup
- Sign your USPTO declaration personally
- Pay your USPTO filing fee directly to USPTO ($750, three classes, TEAS Plus)
- Not represent Forge as having signed any legal filing on your behalf
- Not use Forge to claim a brand identity that infringes on an existing trademark in a related class (we run pre-clearance; you certify our verdict before purchase)

---

## §9. Liability cap

Etymolt's total liability for any claim arising out of these Terms or your use of Forge is limited to the fees you paid us — i.e., **$499 (Lite) or $699 (Pro)**. We are not liable for:

- USPTO examiner decisions on your trademark application
- Platform decisions to suspend, ban, or restrict accounts you created
- Loss of business or revenue from any of the above
- Domain-renewal failures (your responsibility once we hand off)
- Third-party service outages (Cloudflare, Privy, Composio, TextVerified, Stripe)

Disputes over the $750 USPTO fee are between you and USPTO; Etymolt is not a party to that transaction.

---

## §10. Privacy

Forge processes your data minimally:

- **Email address** (yours and the canonical `hello@yourbrand.com` we set up) — stored at Cloudflare KV for 7 days post-receipt; archived to your personal Gmail if you opt in to forwarding
- **Brand name + verdict data** — already covered by Etymolt's main Privacy Policy at [`/privacy`](https://www.etymolt.com/privacy)
- **Stripe payment data** — handled by Stripe; we never see card numbers
- **Generated passwords** — stored in a per-customer credentials vault file; encrypted at rest; delivered to you at handoff; we retain a copy for 30 days for account-recovery support, then delete

We do not sell, share, or transfer your data to third parties beyond the platforms we onboard you to. See [Etymolt Privacy Policy](https://www.etymolt.com/privacy) for details.

---

## §11. Governing law and jurisdiction

These Terms are governed by the laws of the State of Delaware (or the operator's domicile state at the time of dispute), without regard to conflict-of-law principles. Any dispute will be resolved by binding arbitration in that jurisdiction, except where local consumer protection law mandates otherwise.

---

## §12. Changes

We may update these Terms. Material changes will be announced via email to your kickoff-intake address with at least 14 days' notice. Continued use of Forge after the effective date constitutes acceptance.

---

## §13. Contact

- **Operational questions:** [hello@etymolt.com](mailto:hello@etymolt.com)
- **Legal notices:** [legal@etymolt.com](mailto:legal@etymolt.com)
- **Security concerns:** [security@etymolt.com](mailto:security@etymolt.com)

All three currently route to the operator (Tariq A) via Cloudflare Email Routing.

---

*Updated 2026-05-15. Drafted under the 2026-05-15 pre-launch board's mandate (`strategy/boardrooms/2026-05-15_pre-launch-readiness-review.md` action item).*

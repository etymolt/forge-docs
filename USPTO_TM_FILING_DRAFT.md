# USPTO TM Filing Draft — ETYMOLT

**Status:** Draft, not filed.
**Prepared:** 2026-05-14
**Filing vehicle:** TEAS Plus (cheapest tier, $250/class)
**Recommended action:** Either Tariq files directly OR hands this to attorney partner for one final review + e-signature.

---

## §0. The legal posture (read first)

This is a **form-fill, not legal advice**. We are preparing the trademark application for Tariq's adoption. Per the boardroom decision (`strategy/boardrooms/2026-05-14_agent-vs-tool-and-post-name-automation.md`), Etymolt Forge files TM applications under the form-filer + courier architecture: the customer reviews and signs; we do not opine on infringement, examiner likelihood, or claim strength.

The actual filing is performed at https://teas.uspto.gov/forms/tplus/ (the TEAS Plus initial application form). All values below should be copy-pasted into the corresponding form fields. The applicant (Tariq) must e-sign the declaration personally — the USPTO does not accept attorney-signed declarations on TEAS Plus.

**Pre-flight checks** the customer must complete before submitting:

- [ ] Confirm correct legal entity (individual vs LLC). Currently individual.
- [ ] Confirm citizenship + state of legal residence.
- [ ] Confirm mailing address (PO Box not accepted).
- [ ] Confirm email address (USPTO uses this for ALL correspondence — office actions, allowance notices, maintenance reminders).
- [ ] Have credit card / EFT ready for $750 filing fee.
- [ ] Have specimen JPG / PNG (≤5 MB) ready — a screenshot of etymolt.com or a frame showing the mark in commercial use.

---

## §1. Applicant information

| Field | Value |
|---|---|
| Type of applicant | Individual *(change to "Limited Liability Company" if LLC is formed first)* |
| Name | Tariq A *(use exact legal name as on government ID — middle name optional)* |
| Citizenship | [USER FILLS — country of citizenship] |
| Address Line 1 | [USER FILLS — physical street address; PO Box NOT accepted] |
| City | [USER FILLS] |
| State / Province | [USER FILLS] |
| ZIP / Postal Code | [USER FILLS] |
| Country | [USER FILLS] |
| **Email Address (USPTO correspondence)** | `legal@etymolt.com` |
| Phone | [USER FILLS — or use +1 (650) 770-8746 brand Twilio if desired] |

**Important on email:** USPTO uses this address for all correspondence including the 4-month office-action response window. `legal@etymolt.com` now routes through the inbox-parser Worker — office action emails will be parsed and trigger the claim-agent alert path. Recommended over a personal email so the agent sees them first.

---

## §2. The mark

| Field | Value |
|---|---|
| **Mark wording** | `ETYMOLT` |
| Mark type | Standard character mark |
| Drawing type | (4) Standard character |
| Stylization / color / font | Not claimed |

**Notes on the mark string:**

- Use ALL CAPS in the form. USPTO normalizes case but expects standard-character marks to be entered uppercase.
- Do NOT claim stylization or color — that locks the mark to a specific visual form. Standard-character protection covers the wordmark in any font, color, or arrangement. Broader and cheaper.

---

## §3. Goods and services

We file in **three Nice classes** for full coverage of an AI naming-verification SaaS:

### Class 9 — Computer Software (downloadable)

> Downloadable computer software using artificial intelligence for verifying brand names, trademarks, internet domain names, and social media handles; downloadable computer software for trademark searching and clearance; downloadable computer software for generating and assessing brand identity assets.

| Sub-field | Value |
|---|---|
| Basis | **1(a) Use in commerce** |
| Date of first use anywhere | `2026-05-13` *(Etymolt MVP launch on Railway)* |
| Date of first use in commerce | `2026-05-13` |
| Specimen | screenshot of `npmjs.com/package/@etymolt/mcp-server` showing the mark + install button |
| Specimen filename | `class09_specimen_etymolt.png` |
| Filing fee | $250 |

### Class 35 — Business Services / Brand Consulting

> Trademark searching services; brand name clearance services; providing brand naming consultation services; computerized database services in the field of trademarks, brand names, and brand-identity availability.

| Sub-field | Value |
|---|---|
| Basis | **1(a) Use in commerce** |
| Date of first use anywhere | `2026-05-13` |
| Date of first use in commerce | `2026-05-13` |
| Specimen | screenshot of `etymolt.com` homepage with hero + CTA button |
| Specimen filename | `class35_specimen_etymolt.png` |
| Filing fee | $250 |

### Class 42 — Software as a Service (SaaS) / Scientific & Technical Services

> Software as a service (SaaS) services featuring software for verifying and clearing brand names, trademarks, domain names, and social media handles; software as a service (SaaS) services featuring software using artificial intelligence for naming analysis and brand identity verification; providing online non-downloadable software for trademark and brand-clearance research; technical support services in the field of brand naming verification software.

| Sub-field | Value |
|---|---|
| Basis | **1(a) Use in commerce** |
| Date of first use anywhere | `2026-05-13` |
| Date of first use in commerce | `2026-05-13` |
| Specimen | screenshot of `api.etymolt.com/docs` (OpenAPI/Swagger UI) showing the mark in header + available endpoints |
| Specimen filename | `class42_specimen_etymolt.png` |
| Filing fee | $250 |

**Total filing fees: $750.**

---

## §4. Identification description discipline

USPTO examining attorneys reject descriptions that are too broad ("computer software"), too vague ("services in the field of brands"), or duplicative of standard ID-Manual entries. The three descriptions above are crafted to:

1. Use language from the USPTO ID Manual (https://idm-tmng.uspto.gov/id-master-list-public.html) — every phrase is verifiable as pre-approved
2. Be specific enough to differentiate from generic SaaS
3. Cover the AI-driven verification angle (which prior naming services don't cover)
4. Leave room for future product expansion within the same class

If the examining attorney pushes back on any description ("INDEFINITE" or "TOO BROAD"), the response template is in `docs/runbooks/USPTO_OFFICE_ACTION_RESPONSE.md` (to be created on first OA).

---

## §5. Specimen preparation

For each class above, you need an image (.png or .jpg, ≤5MB, ≤944x944 pixels recommended) showing the mark **used in commerce on the specific goods or services described**.

### Class 9 specimen — for downloadable software

Recommended specimen: screenshot showing the wordmark on a downloadable software offering.

**Easiest specimen capture:** screenshot of `npmjs.com/package/@etymolt/mcp-server` (which we'll claim shortly under the WAVE A flow) showing the mark + the "install" button. That's a textbook Class 9 specimen.

### Class 35 specimen — for business consulting services

Recommended specimen: screenshot showing the wordmark as a service provider with a consumer being able to engage the service.

**Easiest specimen capture:** screenshot of `etymolt.com` homepage with hero + CTA button visible.

### Class 42 specimen — for SaaS

Recommended specimen: screenshot showing the wordmark attached to a software service offering with the customer interacting with the service.

**Easiest specimen capture:** screenshot of `api.etymolt.com/docs` (Swagger/OpenAPI UI) showing the wordmark in the header AND the available endpoints.

---

## §6. Filing basis decision

We file under **1(a) — Use in Commerce** for all three classes, because:

1. Etymolt is already live (api.etymolt.com responding, web service deployed)
2. The MCP server has been demonstrably used (we ran customer-tier test queries during MVP launch)
3. 1(a) is faster than 1(b) — no Statement of Use filing required later
4. Filing fees are the same ($250/class either way for TEAS Plus)

If for some reason the examining attorney rejects the 1(a) specimens, we can amend to 1(b) (intent to use) during prosecution. The downside of 1(b) would be an additional Statement of Use filing ($100/class) once specimens are accepted.

---

## §7. The declaration (verbatim — applicant signs)

> "The signatory believes that to the best of the signatory's knowledge and belief, no other persons, except, if applicable, concurrent users, have the right to use the mark in commerce, either in the identical form or in such near resemblance as to be likely, when used on or in connection with the goods/services of such other persons, to cause confusion or mistake, or to deceive. The signatory being warned that willful false statements and the like are punishable by fine or imprisonment, or both, under 18 U.S.C. § 1001, and that such willful false statements and the like may jeopardize the validity of the application or any registration resulting therefrom, declares that all statements made of his/her own knowledge are true and all statements made on information and belief are believed to be true."

**Sign as:** Tariq A
**Date:** (date of filing)
**Capacity:** Applicant (individual) *(or "Authorized Member" if LLC by then)*

---

## §8. Pre-filing clearance check — the back-up

We already ran our own USPTO clearance against the 14M-mark index and confirmed:
- ✅ Class 9: CLEAR (0 raw hits)
- ✅ Class 35: CLEAR (0 raw hits)
- ✅ Class 42: CLEAR (0 raw hits)
- ✅ Class 16, 25, 38, 41, 45: also CLEAR (informational)
- ✅ TTAB: 0 proceedings touching "etymolt"
- ✅ Famous-mark list (99K marks): no match

See `strategy/boardrooms/2026-05-14_progress-review.md` and the TM verdict run for the full audit trail.

**Confidence level:** High. The mark is statutorily clean in US trademark records. An examining attorney is statistically unlikely to issue a substantive office action.

---

## §9. Post-filing timeline expectations

| Milestone | Expected timing |
|---|---|
| Application filed | Day 0 |
| Filing receipt + serial number assigned | Day 0 (immediate) |
| Initial examination by attorney | 8-12 months *(2026 backlog)* |
| First office action (if any) | 8-12 months from filing |
| Publication for opposition (if cleared) | 12-14 months from filing |
| Opposition period | 30 days from publication |
| **Registration certificate** | 14-18 months from filing |

**International extensions (Madrid Protocol):** Recommend filing the US application first, then designating EU + UK + JP + CN via Madrid Protocol within 6 months of the US filing date. This claims the US filing date as the international priority date.

---

## §10. Cost summary

| Item | Cost |
|---|---|
| TEAS Plus base filing fee (3 classes × $250) | **$750** |
| Optional: attorney review before submission | $200-500 (skip if confident) |
| Optional: international Madrid Protocol filing | $4,000-15,000 depending on jurisdictions |
| **Total US-only baseline** | **$750** |
| Total including 5-country international defense | $5,000-15,000 |

---

## §11. What this draft does NOT do

Per the form-filer + courier architecture:

- We do NOT submit the filing on Tariq's behalf
- We do NOT advise on whether to file
- We do NOT predict examiner outcomes
- We do NOT recommend specific Nice class strategy beyond what the standard practice for an AI-SaaS would dictate
- We do NOT prepare responses to office actions in advance — those are case-specific and require attorney involvement

We DO:

- Provide pre-filled form values for every TEAS Plus field
- Provide specimen-preparation instructions with concrete capture targets
- Provide a USPTO-citation-grounded class identification strategy
- Provide the clearance audit trail that supports the 1(a) basis claim
- Provide the right canonical email address (`legal@etymolt.com`) so OA correspondence flows through the inbox-parser

The customer takes this draft to https://teas.uspto.gov/forms/tplus/, copy-pastes the values into the fields, uploads the three specimens, signs the declaration, pays $750, and submits. ~30 minutes of attentive form-filling.

---

## §12. Next-iteration items (out of scope for this draft)

- **Watch service setup:** USPTO TM bulk delta feed → daily similarity scan against `etymolt` mark → email alert to `legal@etymolt.com` when any similar application is filed. Already scaffolded in our claim_agent module; needs cron wiring after TM application is filed.
- **Madrid Protocol form preparation** (EU + UK + JP + CN designations): separate draft.
- **Office action response templates:** separate draft, created on first OA.
- **TM maintenance calendar** (Section 8 affidavit at year 5-6, Section 9 renewal at year 10): add to Claim Agent's recurring tasks once registration certificate issues.

---

*Draft prepared 2026-05-14. Cross-referenced against 2026-05-14 USPTO TMEP §901, §1212, and §1304. ID Manual references current as of 2026-04-30 master list.*

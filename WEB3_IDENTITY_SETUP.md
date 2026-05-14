# Web3 Identity Setup — ETYMOLT

**Status:** Ready to run, awaiting wallet funding.
**Prepared:** 2026-05-14
**Modules:**
- `apps/api/services/claim_agent/web3_wallet.py`
- `apps/api/services/claim_agent/ens.py`
- `apps/api/services/claim_agent/farcaster.py`
- `apps/api/scripts/claim_brand.py` (orchestrator, `web3_identity` platform)

---

## §0. What this does

Brings the etymolt brand identity onchain in three layers:

1. **Brand wallet** — Privy-custodied EOA wallet (TEE-isolated; no private keys
   touch our codebase). Falls back to a local AES-GCM-encrypted key if Privy
   isn't configured.
2. **ENS** — Imports `etymolt.com` as an ENS name via DNSSEC proof
   (one-time onchain proof; no annual ENS fee beyond the DNS renewal). Then
   publishes brand records (eth address, avatar, url, twitter, github,
   description) on the ENS Public Resolver.
3. **Farcaster** — Registers a Farcaster ID (FID) bound to the brand wallet,
   rents 1 storage unit on Base, claims the free `etymolt` fname, and sets
   the profile (display name, bio, pfp, url).

After this runs, `etymolt.com` resolves to `etymolt.eth` in any ENS-aware app,
and `@etymolt` on Farcaster posts from the brand wallet's identity.

---

## §1. Prerequisites

### 1.1 Privy setup (one-time, ~10 min)

1. Sign up at https://console.privy.io with `claims@etymolt.com`
2. Create a new app (name: "Etymolt Forge")
3. Generate an API key pair (App ID + App Secret)
4. Generate an Authorization Key (ed25519) — protects write operations
5. Save into `apps/api/.env`:

```bash
PRIVY_APP_ID=<from dashboard>
PRIVY_APP_SECRET=<from dashboard>
PRIVY_AUTHORIZATION_KEY=<ed25519 private key>
```

Free tier: unlimited dev wallets, 1000 prod wallets, $0.10/wallet after.

### 1.2 Neynar setup (one-time, ~3 min)

Needed for Farcaster profile updates (publishes Hub messages via their managed API):

1. Sign up at https://neynar.com with `claims@etymolt.com`
2. Create a Sponsored Signer (Neynar handles the on-chain signer registration)
3. Save into `apps/api/.env`:

```bash
NEYNAR_API_KEY=<from dashboard>
NEYNAR_SIGNER_UUID=<from sponsored-signers page>
```

Free tier: 1000 requests/day. Sufficient for the dogfood.

### 1.3 DS record at GoDaddy (one-time human step, ~5 min)

ENS DNS-on-chain import requires the parent zone (`.com` registrar = GoDaddy)
to acknowledge our Cloudflare DNSSEC key. The DS record was already captured
on 2026-05-14 by `cf.enable_dnssec("etymolt.com")`:

```
etymolt.com. 3600 IN DS 2371 13 2 53F3E6ED2787108EDA980FA220BEC6FB4F0AD7FE7E745BB6EBA1E4952EA4A6F1
```

To paste at GoDaddy:

1. Log in at https://dcc.godaddy.com/control/portfolio
2. Click `etymolt.com` → Settings → DNSSEC
3. Click "Add DS Record" and fill:
   - Key Tag: `2371`
   - Algorithm: `13` (ECDSAP256SHA256)
   - Digest Type: `2` (SHA-256)
   - Digest: `53F3E6ED2787108EDA980FA220BEC6FB4F0AD7FE7E745BB6EBA1E4952EA4A6F1`
4. Save and wait 10-60 minutes for propagation

Verify with:

```bash
dig +dnssec DS etymolt.com @8.8.8.8 | grep DS
```

When the answer includes our key tag (2371), the chain is established.

### 1.4 Wallet funding (one-time, ~$30-50)

The brand wallet needs ETH on two chains:

- **Ethereum mainnet:** ~0.01 ETH (~$25-30) for ENS proveAndClaim + resolver records
- **Base mainnet:** ~0.005 ETH (~$1-2) for Farcaster FID + storage rent

After `claim_brand.py` creates the wallet, it prints the address. Send funds
via:
- Coinbase / Kraken withdrawal directly to the address
- Or your existing wallet via MetaMask

Funding doesn't need to happen before the script runs — the script will
pause and print the address, then wait for you to confirm funding before
proceeding to the on-chain steps.

---

## §2. Running the full flow

```bash
cd apps/api
python -m scripts.claim_brand etymolt \
    --email claims@etymolt.com \
    --domain etymolt.com \
    --twitter etymolt \
    --github etymolt \
    --platforms web3_identity
```

What happens step-by-step:

1. **Wallet** — creates a Privy server-side wallet OR a local AESGCM-encrypted
   keypair. Prints the address. Persists to `.claim_state/etymolt.json`.

2. **Funding gate** — checks balances on both Ethereum mainnet and Base
   mainnet. If below thresholds, prints funding instructions and waits for
   user to press Enter.

3. **DNS prep** — verifies DNSSEC is active at Cloudflare and writes
   `_ens.etymolt.com TXT "a=0x<wallet_address>"`. If DNSSEC is still pending
   at the parent zone, halts and prints the DS record to paste at GoDaddy.

4. **ENS DNS import** — fetches the DNSSEC proof from `api.ens.domains`,
   then submits `proveAndClaimWithResolver(...)` to the ENS DNSRegistrar.
   One-time tx, ~$15-20 in gas at current Ethereum prices.

5. **Resolver records** — multicall on the Public Resolver to set:
   - `addr` → brand wallet address
   - `text["url"]` → `https://etymolt.com`
   - `text["description"]` → brand description
   - `text["avatar"]` → `https://etymolt.com/brand/etymolt/profile.png`
   - `text["com.twitter"]` → `etymolt`
   - `text["com.github"]` → `etymolt`
   - `text["email"]` → `hello@etymolt.com`

   One tx, ~$5-10 in gas.

6. **Farcaster FID** — calls IdGateway.register() with 1 storage unit on Base.
   Cost: ~0.003 ETH storage + ~$0.05 gas. After ~30s the FID is queryable.

7. **Farcaster fname** — signs EIP-712 typed message and POSTs to
   `fnames.farcaster.xyz/transfers`. Free. Claims `etymolt` as the canonical
   username for the FID.

8. **Farcaster profile** — Neynar API call to set display name, bio, pfp URL,
   and homepage URL.

Final state is persisted to `.claim_state/etymolt.json` for inspection.

---

## §3. Idempotency

Re-running the script is safe:

- **Wallet:** loaded from disk if already created (same address each run).
- **DNS prep:** TXT record only written if missing or stale.
- **ENS import:** the proveAndClaim tx is gated on whether `ens_claim_tx`
  is present in state.
- **Resolver records:** overwrites existing records (idempotent — same result
  every time given the same inputs).
- **Farcaster:** `get_fid_for_address()` is checked first; if the wallet
  already has an FID, registration is skipped and only the profile is
  refreshed.

So if step 6 (Farcaster) fails after step 5 (ENS records) succeeds, re-running
the script picks up at Farcaster without re-doing the ENS tx.

---

## §4. Verifying the result

After a successful run, sanity-check:

### ENS

```bash
# Should return the brand wallet address
curl -sX POST https://eth.llamarpc.com \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0x231b0Ee14048e9dCcD1d247744d114a4EB5E8E63","data":"0x3b3b57de<namehash-of-etymolt.com>"},"latest"],"id":1}'
```

Or in browser: https://app.ens.domains/etymolt.com — should show the brand
wallet as Manager and Controller.

### Farcaster

```bash
# Lookup by fname
curl https://fnames.farcaster.xyz/transfers/current?name=etymolt | jq

# Lookup by FID (after registration)
curl https://api.neynar.com/v2/farcaster/user/bulk?fids=<fid> \
  -H "api_key: $NEYNAR_API_KEY" | jq
```

Or in Warpcast app: search `@etymolt` — should appear with the brand pfp.

---

## §5. What this does NOT do

Per the form-filer + courier architecture:

- We do NOT automate the GoDaddy DS-record paste (no API access on standard tier)
- We do NOT auto-fund the wallet (cold-start funding is user-controlled)
- We do NOT register `etymolt.eth` (separate paid path — call
  `ens.register_eth_name("etymolt", wallet)` after funding the wallet with
  ~0.01 ETH for the registration price)
- We do NOT post the first cast — leaves that as a human authorship moment

---

## §6. Cost summary

| Item | Cost |
|---|---|
| Privy custody (free tier) | $0 |
| Neynar API (free tier) | $0 |
| Cloudflare DNS + Email Routing + Worker | $0 (free tier) |
| Ethereum gas for ENS DNS import | ~$15-20 |
| Ethereum gas for resolver records | ~$5-10 |
| Base gas + storage rent for Farcaster | ~$3-5 |
| **Total one-time onchain costs** | **~$25-35** |
| Recurring (etymolt.com renewal at GoDaddy) | ~$13/year |
| Recurring (Cloudflare, Privy, Neynar) | $0 (within free tiers) |

Compare to the alternative: $5/yr for `.eth` *AND* $13/yr for `.com`. The
DNS-import path gives us ENS protection on the same domain we already paid
for, no second annual fee.

---

## §7. Open follow-ups

- [ ] Wire `set_resolver` separately from `proveAndClaimWithResolver` in case
      we need to migrate to a newer resolver version
- [ ] Build the `register_eth_name_commit` / `register_eth_name_finalize`
      pair for the paid `.eth` path (currently stubbed)
- [ ] Add Hub signer registration so we can post casts without Neynar (full
      self-custody path) — needed for the production product, not the dogfood
- [ ] Add the bundler (single-tx FID + storage + key) once Farcaster's docs
      stabilize on the metadata format

*Draft prepared 2026-05-14. Ready to execute as soon as Privy onboarding + GoDaddy DS paste + wallet funding are complete.*

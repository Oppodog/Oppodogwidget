# Jupiter Ultra Integration Details

This document expands on how the Oppo widget scaffold integrates (or will integrate) with Jupiter Ultra referral fee mechanics.

## Goals
- Correctly apply a 0.2% referral fee (20 bps) on supported premium tokens
- Present transparent fee + status to end users
- Track unclaimed referral earnings & auto-claim when threshold met
- Provide a modular surface (window.OppoConfig) for future framework bindings

## Current State (Scaffold)
The repository presently includes a static `index.html` that:
- Loads the Jupiter widget via `<script src="https://plugin.jup.ag/plugin-v1.js"></script>`
- Supplies `feeBps` & `feeRecipient`
- Stubs `onSwapSuccess` to simulate fee accrual using local (in‑memory) state
- Exposes helper functions for future integration logic

No real on-chain validation of transactions is performed yet. This is intentional for rapid UI iteration.

## Planned Production Flow
1. User swaps through Jupiter widget
2. `onSwapSuccess` receives a transaction signature
3. A backend (or secure edge function) validates:
   - The swap actually occurred
   - The referral fee was embedded
   - Token mints involved are in supported premium list
4. Backend records & indexes referral accrual (DB / KV / on-chain indexer)
5. Aggregated unclaimed value returned to client periodically
6. Auto-claim service triggers when >= $1 (or configurable) threshold AND account verification passes

## Fee Configuration
```ts
interface ReferralConfig {
  feeBps: number;          // 20 = 0.2%
  feeRecipient: string;    // Referral public key
  supportedMints: Map<string,string>; // mint -> symbol
}
```

## Supported Premium Tokens
(See README for list.) Maintain a single source of truth. In production this list should be **fetched** (e.g. from a signed JSON or versioned endpoint) to allow updates without redeploy.

## Security Considerations
| Vector | Risk | Mitigation (planned) |
|-------|------|----------------------|
| Fake client fee claims | Users forging local state | All authoritative accounting occurs server-side; client only displays. |
| Tampered feeRecipient | Malicious script override | Hard-code & also attest via backend; optionally integrity meta tag or Subresource Integrity for widget loader. |
| Token list spoofing | Trick UI into showing unsupported assets | Signed token list + hash validation. |
| Over-claim / replay | Double crediting same swap | Store processed tx signatures (idempotency). |

## Auto-Claim Strategy
- Passive polling: client periodically fetches aggregated unclaimed amount (e.g. every 60s)
- If `unclaimed >= 1 USD` AND `accountVerified=true`, trigger claim mutation
- Claim transaction built & sent server-side (reduces key exposure)
- UI receives result + new balance

## Demo Mode
The current scaffold simulates: random increments of $0.10 – $0.50 per simulated or successful swap. This allows UI prototyping without RPC or backend.

## Future Enhancements
- Formal TypeScript SDK wrapper (`@oppo-widget/referral`)
- WASM or worker-based transaction parsing
- Real-time push (WebSocket / SSE) for fee accrual
- Multi-wallet session abstraction
- Metrics & observability hooks (OTel events)

## Reference
- Jupiter Docs: https://docs.jup.ag/
- Ultra Fee Guide: https://dev.jup.ag/docs/ultra-api/add-fees-to-ultra

---
For architectural discussions open an issue with the label `architecture`.
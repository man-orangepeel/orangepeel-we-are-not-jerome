# Reward Flow

The complete end-to-end flow from a child completing a challenge to sats arriving in their wallet.

***

## Six steps

| Step | Actor      | Action                                                            |
| ---- | ---------- | ----------------------------------------------------------------- |
| 1    | Child      | Completes challenge → taps "I did it"                             |
| 2    | Child app  | Generates Lightning invoice for X sats (Breez SDK)                |
| 3    | Backend    | Receives invoice → sends push notification to parents             |
| 4    | Parent     | Validates in one tap — or auto-signed if ≤ threshold              |
| 5    | Parent app | Breez SDK signs and broadcasts tx (local key, no server involved) |
| 6    | Child      | Receives X sats in self-custody wallet                            |

***

## Diagram

![Reward flow](../.gitbook/assets/3-reward-flow.png)

***

## Child interface

The child's app is adapted by age group (3–6 / 7–11 / 12–15 / 16–18):

* Balance in sats (primary) + EUR spot equivalent
* Active missions + reward shown upfront
* "I did it" button — triggers the payment request
* History: every sat has a story

***

## Auto-sign

If the reward amount is at or below the parent-defined threshold, step 4 is skipped entirely. The parent wallet signs automatically. No notification sent, no tap required.

Parents set the threshold during onboarding and can adjust it at any time.

## Magic Routing

If both parent and child are on the app (both using Breez SDK / Liquid):

* Direct Liquid transaction
* \~26 sats fee
* Settles in seconds

Otherwise: standard Lightning payment, routed via Boltz swap if needed.

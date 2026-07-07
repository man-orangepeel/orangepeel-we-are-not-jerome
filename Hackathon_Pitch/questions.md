# Open Questions

This is where we need the team. These are genuine protocol and architecture questions — not rhetorical.

---

## 1. Payment lock

Two parents share the same seed on two phones. How do we guarantee a Lightning invoice is only signed once?

A backend lock (mark invoice as claimed on first validation request) seems obvious. But is there a more elegant on-device solution that doesn't require a server round-trip?

---

## 2. BIP-85 UX

Seed derivation must happen on-device. What's the right moment in onboarding to derive child wallets?

- **Lazy** — derive on first child creation (lower initial friction, but requires the seed to be accessible later)
- **Eager** — derive all at setup (simpler key lifecycle, but requires committing to child count upfront)

---

## 3. LNURL fee split

We want the fee taken before funds reach the parent wallet. Does Breez SDK's LNURL-pay support a split natively, or do we need a proxy server in front?

If a proxy server is needed: what does it look like, and does it introduce any custodial surface?

---

## 4. Auto-sign threshold

The signing key lives on-device (Breez SDK). How do we enforce the threshold rule without a backend round-trip?

Can Breez SDK be configured to auto-pay invoices under X sats? Or does this require a local policy layer that intercepts Breez SDK calls?

---

## 5. NWC in 48h

Breez SDK exposes NWC natively. How much work to:

1. Expose a per-child NWC endpoint
2. Apply parental spend caps (per-tx and daily)
3. Demo it live with an external NWC-compatible tool

Is this a 2-hour add-on or a 1-day project?

---

## 6. React Native + Breez SDK

Breez SDK is Rust/FFI. What's the current state of React Native bindings?

- Bare React Native setup or Expo with a custom dev client?
- Known issues on iOS / Android?
- Any team members with prior Breez SDK + RN experience?

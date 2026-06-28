# MVP Scope (48h)

What we commit to shipping during the hackathon. Everything else is post-hackathon.

---

## In scope

- [ ] Parent wallet — BIP-39 seed, Breez SDK, balance display (sats + EUR)
- [ ] 1 child wallet — BIP-85 derivation at index 0
- [ ] 1 hardcoded challenge + "I did it" button
- [ ] Reward flow: Lightning invoice → parent push notification → validation → payout
- [ ] Auto-sign if reward ≤ threshold set by parent
- [ ] Balance display: sats + EUR spot equivalent

---

## If time allows

**NWC endpoint per child wallet.**
Breez SDK exposes Nostr Wallet Connect natively — low marginal effort, strong ecosystem demo. Allows any NWC-compatible partner to send sats to a child without a specific integration.

---

## Out of scope for MVP

- Multiple children (multi-index BIP-85)
- Fiat onramp (Transak)
- Leaderboard / points system
- Collective missions
- Tricky / Investisseur / Entreprise modes
- App store submission

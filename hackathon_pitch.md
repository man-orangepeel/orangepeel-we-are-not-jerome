# UNPRINTED — Hackathon Pitch
> *"Proof of work starts at home."*

---

## The problem

Bitcoin parents criticize monetary printing — then give their kids unconditional fiat allowance every month. No effort. No value created. Jerome Powell at home.

Existing tools (Greenlight, Pixpay) replicate the same model: bank custody, subscriptions, no real financial education. Every "Bitcoin education app" hits the same wall: who funds the rewards? We solve it — **parents already pay. We just fix the flow.**

---

## What it does

Kids earn sats by completing real challenges — chores, quizzes, physical goals. Parent validates in one tap. Sats land in the kid's own self-custody wallet, derived from the family seed.

- One BIP-39 parent seed → N child wallets via BIP-85 derivation. One backup for the whole family.
- 1–2% fee on parent deposits via LNURL split — non-custodial, MiCA-exempt, no subscription.
- Auto-sign: if the reward is below a parent-defined threshold, the tx fires without manual approval.
- Magic Routing: if both parent and child are on the app → direct Liquid tx (~26 sats fee). Otherwise standard Lightning swap.

---

## MVP scope (48h)

- [ ] Parent wallet (BIP-39 seed, Breez SDK)
- [ ] 1 child wallet (BIP-85 index 0)
- [ ] 1 hardcoded challenge + "I did it" button
- [ ] Reward flow: Lightning invoice → parent notification → validation → payout
- [ ] Auto-sign if ≤ threshold
- [ ] Balance display: sats + EUR spot equivalent

**If time allows:** NWC endpoint per child wallet (Breez SDK exposes this natively — low marginal effort, strong ecosystem demo).

---

## Proposed stack

These are starting assumptions, not mandates — open to better ideas:

| Layer | Choice | Why |
|---|---|---|
| Mobile | React Native | iOS + Android from one codebase |
| Bitcoin / wallet | Breez SDK Nodeless | Self-custody, Lightning + Liquid + NWC, open-source |
| Seed architecture | BIP-39 + BIP-85 | 1 parent seed → N child wallets, 1 family backup |
| Fee split | LNURL-pay | Fee taken at protocol level before funds hit the wallet |
| Swaps | Boltz | LN ↔ L1 ↔ Liquid atomic swaps |
| Fiat onramp | Transak | CB → sats, KYC delegated, revenue share |
| Push | FCM / APNs | Parent validation notifications |
| Backend | Lightweight (Node or Python) | LNURL server, payment lock, push relay |

---

## Open technical questions

This is where we need the team:

1. **Payment lock** — two parents share the same seed on two phones. How do we guarantee an invoice is only signed once? Backend lock seems obvious, but is there a more elegant on-device solution?

2. **BIP-85 UX** — seed derivation must happen on-device. What's the right moment in onboarding to derive child wallets? Lazy (on first child creation) or eager (all at setup)?

3. **LNURL fee split** — we want the fee taken before funds reach the parent wallet. Does Breez SDK's LNURL-pay support split natively, or do we need a proxy server?

4. **Auto-sign threshold** — the signing key lives on-device (Breez SDK). How do we enforce the threshold rule without a backend round-trip? Can Breez SDK be configured to auto-pay invoices under X sats?

5. **NWC in 48h** — Breez SDK exposes NWC natively. How much work to expose a per-child endpoint, apply parental spend caps, and demo it live?

6. **React Native + Breez SDK** — Breez SDK is Rust/FFI. What's the current state of RN bindings? Do we start with a bare RN setup or Expo with a custom dev client?

---

## What we're looking for

| Role | Skills |
|---|---|
| Mobile dev | React Native, ideally some Lightning/Bitcoin experience |
| Bitcoin/protocol dev | Breez SDK, BIP-85/39, LNURL, Liquid — the deeper the better |
| Backend dev | Node or Python, LNURL server, webhook/push infra |
| UX/Design | Mobile-first, ideally experience with child-facing products |
| Biz / Growth | Business model, Go To Market, branding, marketing

---

## Why this is worth 48h

- Non-custodial from day one → MiCA-exempt, no license needed
- Business model that doesn't require VC to bootstrap (fees on existing spend)
- Breez SDK does the heavy lifting on the wallet layer
- The hardest problems are protocol design, not raw engineering volume
- The campaign (*I'm not Jerome*) is a genuine cultural hook in the Bitcoin space

---

*"No work, no sat."*

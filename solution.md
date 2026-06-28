# The Solution

Kids earn sats by completing real challenges — chores, quizzes, physical goals. Parent validates in one tap. Sats land in the kid's own self-custody wallet, derived from the family seed.

---

## How it works

**One seed. The whole family.**
One BIP-39 parent seed derives N child wallets via BIP-85. Index 0 = child 1, index 1 = child 2, and so on. One backup covers everyone.

**Non-custodial from day one.**
The app never touches funds. The fee (1–2%) is taken at the protocol level via LNURL split *before* sats reach the parent wallet. No custody = MiCA-exempt.

**Smart signing.**
If the reward is below a parent-defined threshold, the transaction fires automatically — no tap required. Above the threshold, parents get a push notification and validate in one tap.

**Magic Routing.**
If both parent and child are on the app: direct Liquid transaction (~26 sats, a few seconds). Otherwise: standard Lightning swap via Boltz.

---

## The flow in six steps

1. Child completes a challenge → marks it "done" in the app
2. Child's app generates a Lightning invoice for X sats
3. Backend receives the invoice → notifies both parents (push)
4. Parent validates (one tap) — or auto-sign if ≤ threshold
5. Breez SDK signs the tx from the parent wallet (local key)
6. Child receives X sats in their own self-custody wallet

→ See the full [reward flow diagram](architecture/reward-flow.md)

---

## Business model

- **1–2% fee** on every parent deposit via LNURL split — collected before funds arrive, never held by us
- **~0.3–0.5% revenue share** from Transak on fiat onramp (card → sats)
- No subscription. No VC needed to bootstrap.

# Collective Rewards

Some missions can be completed as a team — all children in the family work together toward a shared goal. The educational lesson: coordination, collective responsibility, and the value of the group.

---

## How it works

Collective missions are **same-family only** — all participants must be on the app with BIP-85-derived wallets.

The mode is **collaborative only**: every child must complete their part. Once all children have marked the mission done and the parent validates, a single **Liquid multi-output transaction** pays all children simultaneously.

This is atomic: either everyone gets paid, or nobody does.

---

## Diagram

![Collective reward flow](../assets/4-collective-flow.svg)

---

## Constraints

- Same-family wallets only (all BIP-85 derived from the same parent seed)
- Collaborative mode only at launch — no external children, no sequential Lightning payments for this case
- Parallel mode (children complete independently, paid as they go): planned for a later phase
- Competitive mode: not in scope

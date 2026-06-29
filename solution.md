# The Solution

Kids earn sats by completing real challenges. Parent validates in one tap. Sats land in the kid's own self-custody wallet.

---

## What kids actually do

Four mission categories, adapted by age (3–6 / 7–11 / 12–15 / 16–18):

| Category | Example missions |
|---|---|
| 💪 Taking care of myself | 30 min of sport 3x this week · 7 days without added sugar · consistent sleep routine |
| 🧠 Thinking for myself | Explain what a cookie is · spot a hidden ad · hold an argument at the dinner table |
| ₿ Understanding money | Explain Bitcoin's 21M cap · install a test wallet · understand compound interest |
| 🛠️ Creating value | Help a neighbor · write your first invoice · grow something, fix something, cook something |

Each sat earned has a traceable story. Kids see not just a balance — but a history of what they built.

---

## Four types of missions

| Type | Source | Leaderboard points | Funded by |
|---|---|---|---|
| 📋 Family catalogue | App — predefined list | ❌ | Parent pot |
| ✏️ Custom (parent) | Parent creates freely | ❌ | Parent pot |
| 🌐 Open mission | App — personal progression | ✅ | App pool |
| ⭐ Exclusive mission | App — top 3 weekly | ✅ | App pool |

Parents choose from the catalogue or create their own. The app pool funds missions independently — no extra parent budget required for those.

---

## How it works

**One seed. The whole family.**
One BIP-39 parent seed derives N child wallets via BIP-85. One backup covers everyone.

**Non-custodial from day one.**
The app never touches funds. The fee (1–2%) is taken at the protocol level via LNURL split *before* sats reach the parent wallet. No custody = MiCA-exempt.

**Smart signing.**
If the reward is below a parent-defined threshold, the transaction fires automatically — no tap required. Above the threshold, parents get a push notification and validate in one tap.

**Magic Routing.**
If both parent and child are on the app: direct Liquid transaction (~26 sats, a few seconds). Otherwise: standard Lightning swap via Boltz.

---

## The reward flow in six steps

1. Child completes a challenge → taps "I did it"
2. Child's app generates a Lightning invoice for X sats
3. Backend receives the invoice → notifies both parents (push)
4. Parent validates (one tap) — or auto-sign if ≤ threshold
5. Breez SDK signs the tx from the parent wallet (local key)
6. Child receives X sats in their own self-custody wallet

→ See the full [reward flow diagram](architecture/reward-flow.md)

---

## How UNPRINTED compares

| | UNPRINTED | BTCBitByBit | Greenlight / Pixpay |
|---|---|---|---|
| Self-custody | ✅ day 1 (BIP-85) | ❌ custodial | ❌ bank custody |
| Business model | % on deposits · no subscription | Subscription | Subscription |
| Education scope | Bitcoin + body + critical thinking + entrepreneurship | Bitcoin only | Basic allowance |
| MiCA compliance | ✅ non-custodial exempt | ❓ | ✅ (bank partner) |
| Multi-child | ✅ 1 seed → N wallets | ❓ | ✅ |
| Lightning | ✅ Breez SDK | ❓ | ❌ |
| NWC open platform | ✅ | ❌ | ❌ |
| Collective missions | ✅ Liquid multi-outputs | ❌ | ❌ |

---

## Business model

No subscription. Parents pay only when they fund the pot.

| Source | Mechanism | Rate |
|---|---|---|
| Fee on parent deposits | LNURL split · taken before receipt · non-custodial | 1–2% |
| Transak revenue share | Fiat onramp partnership (card → sats) | ~0.3–0.5% of card volume |

**Example:** 1,000 families × €50/month × 1.5% = **€750/month at 1,000 families**. No VC needed to reach that.

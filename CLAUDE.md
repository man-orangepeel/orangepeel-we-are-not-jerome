# CLAUDE.md — UNPRINTED (campagne "We are not Jerome")

> Fichier de contexte projet pour Claude Code. Lu automatiquement à chaque session.

---

## Projet

App mobile familiale Bitcoin éducative. Les enfants gagnent des sats réels en accomplissant des défis (tâches, quiz, missions). Les parents alimentent un wallet self-custody et valident les récompenses.

**Specs complètes** → `specs_produit.md` (lire en priorité)  
**Architecture** → `1. architecture_fonctionnelle.png` + `stack_technique.png`  
**Flux financiers** → `2. flux_alimentation_bip85.png` + `3. flux_reward_enfant.png`  
**Pitch** → `pitch_presentation.md`

---

## Stack technique

```
React Native (iOS + Android)
Breez SDK Nodeless — wallet self-custody · Lightning · Liquid · on-chain
BIP-39 (seed parent) + BIP-85 (dérivation seeds enfants)
LNURL fee split — fee prélevée avant réception (non-custodial)
Boltz — atomic swaps LN ↔ L1 ↔ Liquid
Transak — fiat onramp (CB → sats)
FCM / APNs — notifications push
Backend léger — LNURL server · push notifs · verrou paiement
```

---

## Contraintes critiques

**Non-custodial absolu.** L'app ne touche jamais les fonds. La fee est prélevée par le protocole LNURL *avant* que les sats arrivent dans le wallet parent. Ne jamais implémenter de logique qui implique que le backend détient des fonds — cela sortirait du périmètre MiCA CASP.

**BIP-85 multi-enfants.** Une seed parent BIP-39 → N seeds enfants dérivées par index. Un seul backup suffit pour toute la famille. Chaque enfant a son propre wallet self-custody.

**Verrou paiement.** Deux parents peuvent valider (même seed, 2 téléphones). Le backend garantit qu'une invoice Lightning n'est signée qu'une seule fois. Aucun double-paiement possible.

**Auto-sign.** Si le montant de la récompense ≤ plafond défini par le parent, la tx est signée automatiquement sans action requise.

**Magic Routing Hints.** Si parent et enfant utilisent tous les deux l'app : transaction Liquid directe (~26 sats de fee, quelques secondes). Sinon : swap Lightning classique.

---

## Architecture wallet

```
Parent A (master BIP-39)     Parent B
  pot : X sats               même seed · lecture + validation

         ↓ BIP-85 dérivation

  Enfant 1      Enfant 2      Enfant N
  index 0       index 1       index N
  self-custody  self-custody  self-custody
```

---

## Flux reward (ordre exact)

1. Enfant complète tâche / QCM → marque "fait" dans l'app
2. App enfant génère une invoice Lightning (Breez SDK) pour X sats
3. Backend reçoit l'invoice → notifie les 2 parents (push)
4. Parent valide (1 tap) OU auto-sign si ≤ plafond
5. Breez SDK signe la tx depuis le wallet parent (clé locale)
6. Enfant reçoit X sats dans son wallet (~26 sats de fee Lightning)

---

## Modèle économique

- Fee 1–2% prélevée à chaque dépôt parent via LNURL split
- Revenue share Transak ~0.3–0.5% sur les dépôts CB
- Aucun abonnement
- Pool app = fees collectées → finance les coûts business + récompenses missions app

---

## Taxonomie des missions — règles importantes

Quatre types de missions, deux sources de financement :

| Type | Label | Points | Sats | Financement |
|---|---|---|---|---|
| Mission ouverte (app) | 🌐 | ✅ | ✅ | Pool app |
| Mission exclusive (app, top 3) | ⭐ | ✅ | ✅ | Pool app |
| Catalogue famille (app, choisi par parent) | 📋 | ❌ | ✅ | Pot parent |
| Custom parent (ajouté librement) | ✏️ | ❌ | ✅ | Pot parent |

- Les parents ne peuvent jamais générer de points — uniquement ajuster les sats
- Le contenu des missions custom ne remonte jamais automatiquement (privacy) — opt-in uniquement
- Les missions ouvertes 🌐 se débloquent par progression personnelle (pas par classement) → anti-circularité
- Les missions exclusives ⭐ sont un bonus réservé au top 3 hebdo, pas la porte d'entrée

---

## Nostr Wallet Connect (NWC)

- App expose un endpoint NWC par wallet enfant (Breez SDK natif)
- Permet à tout partenaire externe NWC-compatible d'envoyer des sats à un enfant sans intégration spécifique
- Plafond par tx et plafond journalier configurables par le parent
- Phase 1 — argument écosystème fort pour hackathon

## Missions collectives — contraintes

- Même famille uniquement (tous sur l'app, wallets Liquid BIP-85 dérivés)
- Mode collaboratif uniquement : tous les enfants complètent → 1 tx Liquid multi-outputs atomique
- Pas d'enfant externe, pas de Lightning séquentiel pour ce cas
- Mode parallèle : phase ultérieure · Mode compétitif : abandonné

## Modes optionnels (choix parent au setup)

- **Normal** : récompense versée immédiatement à la validation
- **Tricky** : incertitude simulée (délais, oublis, bonus surprise, perte sèche)
- **Investisseur** : blocage sats / taux d'intérêt simulés
- **Mode Entreprise** : devis → contrat → facture → charges → crédit → bonus marge

---

## Cadre légal

- Non-custodial → hors périmètre MiCA CASP (Article 3(1)(15))
- Pas de licence CASP requise
- KYC délégué à Transak (partie fiat uniquement)
- RGPD : données mineurs → consentement parental · leaderboard pseudonyme uniquement
- Structure cible : SAS française ou BV néerlandaise

---

## Roadmap actuelle

| Phase | Statut | Objectif |
|---|---|---|
| MVP Hackathon | 🔴 En cours | Wallet parent + 1 enfant + 1 défi + reward auto-signé |
| Beta M1–M3 | ⬜ | Multi-enfants, modules, Transak, Bitrefill, Tricky |
| Launch M4–M6 | ⬜ | App stores, compétition, Mode Entreprise, 100 familles |

---

## Fichiers du projet

```
CLAUDE.md                       ← ce fichier
specs_produit.md                ← specs fonctionnelles complètes (v0.2)
pitch_presentation.md           ← pitch hackathon
1. architecture_fonctionnelle.png
2. flux_alimentation_bip85.png
3. flux_reward_enfant.png
stack_technique.png
```

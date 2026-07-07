# UNPRINTED — Pitch Hackathon
> *"Proof of work starts at home."*  
> Campagne de lancement : **We are not Jerome**

---

## HOOK — 5 secondes

> *"La prochaine génération de bitcoiners n'existe pas encore.  
> C'est votre job de la créer — et vous êtes en train de la rater."*

---

## ARGUMENT 1 — L'enjeu existentiel

**"Orange pill un adulte. Bonne chance."**

Les bitcoiners savent que l'adoption de masse ne viendra pas des institutions, des ETF, des gouvernements. Elle viendra de la génération qui grandit *avec* ces valeurs — pas de celle qui doit les désapprendre.

Orange pill un adulte : des années de conditionnement fiat à défaire. Une bataille épuisante.  
Orange pill un enfant de 8 ans : une fondation pour 70 ans de compréhension saine de la monnaie.

Les personnes les plus importantes à convaincre ne lisent pas Twitter. Elles vivent chez vous.  
La génération qui aura 20 ans quand Bitcoin sera à maturité a entre 5 et 15 ans **aujourd'hui.**

---

## ARGUMENT 2 — La contradiction

**"Vous critiquez la Fed. Vous faites pareil."**

71% des parents américains donnent de l'argent de poche à leurs enfants (Wells Fargo, 2025).  
53% des parents français. Moyenne : 30€/mois en Europe, $37/semaine aux USA.  
Sans effort. Sans valeur apportée. Sans contrepartie.

De l'argent qui tombe du ciel, tous les mois — comme des droits acquis.

Vous avez passé des heures à expliquer pourquoi l'impression monétaire détruit la valeur du travail.  
Puis le 1er du mois, vous faites exactement la même chose avec vos enfants.

Ce n'est pas un reproche. C'est un problème structurel : **aucun outil n'existait pour faire autrement.**

> *"Le parent bitcoiner qui donne 30€/mois à son fils sans contrepartie,  
> c'est Jerome Powell à la maison."*

---

## ARGUMENT 3 — Pourquoi les autres solutions échouent

**"Le cold start problem tue toutes les apps éducatives Bitcoin."**

Toutes les apps qui essaient de rémunérer des nocoiners pour apprendre Bitcoin font face au même mur : qui finance les récompenses ? Un pool externe s'épuise. Un abonnement crée de la friction. Le modèle ne scale pas.

Notre réponse : **les parents paient déjà.**

Ils versent 30–160€/mois à leurs enfants. Pas de budget supplémentaire demandé — on transforme ce flux existant en sats mérités, sur un wallet self-custody, avec une fee de 1–2% qui finance le business.

Le cold start problem n'existe pas ici. Les parents sont la source de financement naturelle et motivée — parce que c'est *leur* enfant, et *leurs* valeurs.

> *"On ne crée pas un nouveau flux d'argent.  
> On répare celui qui existe déjà."*

---

## SOLUTION — UNPRINTED

**Le premier wallet Bitcoin familial qui transforme l'argent de poche fiat en sats mérités.**

Chaque satoshi est gagné par un effort réel, validé par le parent, versé instantanément sur un wallet self-custody dérivé de la seed familiale — sans intermédiaire, sans custody, hors périmètre MiCA.

### Ce qui nous différencie

| Critère | UNPRINTED | BTCBitByBit | Greenlight / Pixpay |
|---|---|---|---|
| Self-custody | ✅ dès le jour 1 (BIP-85) | ❌ custody | ❌ custody bancaire |
| Business model | % sur dépôts · aucun abonnement | Abonnement | Abonnement |
| Éducation | Bitcoin + corps + pensée critique + entrepreneuriat | Bitcoin only | Argent de poche basique |
| MiCA compliance | ✅ non-custodial exempt | ❓ | ✅ (banque partenaire) |
| Multi-enfants | ✅ 1 seed → N wallets (BIP-85) | ❓ | ✅ |
| Lightning natif | ✅ Breez SDK | ❓ | ❌ |
| NWC open platform | ✅ | ❌ | ❌ |
| Missions collectives | ✅ Liquid multi-outputs | ❌ | ❌ |

---

## FONCTIONNEMENT

### Côté parent — onboarding 5 min
1. L'app génère une seed BIP-39 (12 mots) → wallet Lightning / Liquid / on-chain
2. Guidage backup physique pas-à-pas
3. Création des profils enfants : seed BIP-85 dérivée automatiquement par index
4. Définition des règles : montant max auto-signé, modules actifs, mode de jeu

### Alimentation du pot parent
- **CB** : via Transak (€ → sats, ~1.5% frais)
- **Lightning / on-chain** : depuis n'importe quel wallet externe
- **Tiers** : grands-parents, proches — envoi direct sur l'adresse Lightning parent

### Flux reward (6 étapes, quelques secondes)
```
Enfant complète → Invoice LN générée → Backend notifie parents → Parent valide (ou auto-sign)
→ Breez SDK signe depuis wallet parent → Enfant reçoit dans son wallet self-custody
```
> Magic Routing Hints : si parent + enfant sont tous les deux sur l'app → tx Liquid directe (~26 sats de fee, instantanée)

### Côté enfant (interface adaptée par tranche d'âge : 3-6 / 7-11 / 12-15 / 16-18)
- Solde en sats (dominant) + équivalent € au cours spot
- Missions en cours + récompense associée
- Bouton "J'ai réussi" → déclenche la demande de paiement
- Historique : chaque sat a une histoire

---

## MODULES D'ÉDUCATION

Quatre catégories. Missions réelles, vérifiées par le parent ou numériquement par l'app.

| Catégorie | Exemples de missions |
|---|---|
| 💪 Je prends soin de moi | 30 min de sport 3x/semaine · 7 jours sans sucre ajouté · routine de sommeil |
| 🧠 Je pense par moi-même | Expliquer ce qu'est un cookie · identifier une pub cachée · débattre à table |
| ₿ Je comprends mon argent | Expliquer les 21M de BTC · installer un wallet de test · comprendre l'intérêt composé |
| 🛠️ Je crée de la valeur | Rendre service · générer sa première facture · cultiver, réparer, cuisiner |

### Quatre types de missions

| Label | Source | Points classement | Financement |
|---|---|---|---|
| 📋 Catalogue famille | App (prédéfini) | ❌ | Pot parent |
| ✏️ Custom parent | Parent libre | ❌ | Pot parent |
| 🌐 Mission ouverte | App (progression perso) | ✅ | Pool app |
| ⭐ Mission exclusive | App (top 3 hebdo) | ✅ | Pool app |

---

## FEATURES DIFFÉRENCIANTES

### Nostr Wallet Connect — plateforme ouverte
L'app expose un endpoint NWC par wallet enfant. N'importe quel quiz Bitcoin, jeu éducatif, ou partenaire externe peut envoyer des sats à un enfant sans intégration spécifique.

UNPRINTED devient une **plateforme ouverte** — pas seulement une app familiale.

### Missions collectives — Liquid multi-outputs
Un seul paiement parent → splitté atomiquement vers plusieurs enfants.  
1 tx Liquid, N outputs, instantanée. Enseignement : coordination, responsabilité collective, valeur du groupe.

### Mode Tricky (optionnel)
Simule l'incertitude de la vraie vie : paiement retardé, oublié, doublé surprise ou non versé.  
Comme les modes dans Minecraft — activable par le parent.

### Classement & compétition (opt-in Phase 2)
- Pseudonyme + avatar — zéro donnée personnelle
- Classement hebdo (reset lundi) + all-time (points cumulatifs)
- Top 3 → accès prioritaire aux missions exclusives ⭐ (mécanique "marché des opportunités")

### Mode Entreprise (Phase 2)
Devis → contrat → facture → charges → crédit → pénalité de défaut.  
L'enfant gère une micro-entreprise en sats réels.

---

## MODÈLE ÉCONOMIQUE

**Aucun abonnement.** Les parents paient uniquement quand ils alimentent le pot.

| Source | Mécanisme | Estimation |
|---|---|---|
| Fee sur dépôt parent | LNURL-pay split · prélevé avant réception · non-custodial | 1–2% |
| Revenue share Transak | Partenariat fiat onramp | ~0.3–0.5% du volume CB |

**Exemple :** 1 000 familles × 50€/mois × 1.5% = **750€/mois dès 1 000 familles**

Pool app = fees collectées → finance les coûts business + récompenses missions ouvertes et exclusives.

---

## ARCHITECTURE TECHNIQUE

```
React Native (iOS + Android)
    ↓
Logique métier (on-device + backend léger)
  BIP-85 dérivation · auto-sign · verrou paiement · LNURL fee split · NWC endpoints
    ↓
Breez SDK Nodeless — self-custody · Lightning · Liquid · on-chain · NWC natif
    ↓
Partenaires : Boltz (atomic swaps) · Transak (onramp fiat) · FCM/APNs (push)
    ↓
Réseaux : ⚡ Lightning · 🌊 Liquid · ₿ Bitcoin L1
```

**Points clés :**
- **BIP-85** : 1 seed parent → N seeds enfants · 1 seul backup pour toute la famille
- **LNURL split** : fee prélevée au niveau protocole — l'app ne touche jamais les fonds
- **Liquid multi-outputs** : 1 tx pour payer N enfants simultanément (missions collectives)
- **NWC** : natif Breez SDK — ouverture à tout l'écosystème compatible

---

## CADRE LÉGAL

**Non-custodial = hors périmètre MiCA CASP.**

> MiCA Article 3(1)(15) : un CASP est un prestataire qui *détient* des crypto-actifs. UNPRINTED ne détient rien — les clés restent sur les devices des utilisateurs.

- Pas de licence CASP requise
- KYC délégué à Transak (partie fiat uniquement)
- RGPD : données mineurs → consentement parental · leaderboard pseudonyme uniquement
- Structure : SAS française ou BV néerlandaise

---

## ROADMAP

| Phase | Statut | Scope |
|---|---|---|
| **MVP Hackathon** | 🔴 En cours | Wallet parent + 1 enfant + 1 défi + reward auto-signé + NWC demo |
| **Beta M1–M3** | ⬜ | Multi-enfants · catalogue missions · missions collectives · Transak · Bitrefill · Tricky |
| **Launch M4–M6** | ⬜ | App stores · compétition · mode Entreprise · 100 familles beta |

---

## BRAND

**Nom :** UNPRINTED  
**Tagline :** *"Proof of work starts at home."*  
**Campagne de lancement :** *We are not Jerome*

> *"Le parent bitcoiner qui donne 30€/mois à son fils sans contrepartie,  
> c'est Jerome Powell à la maison.  
> We are not Jerome. Our kids aren't either."*

**Autres slogans :**
- *"Your kids can't be printed."*
- *"Sound money. Sound kids."*
- *"Character is forged, not given."*
- *"No work, no sat."*

---

## SOURCES

- Wells Fargo (2025) : 71% des parents US donnent une allowance · moyenne $37/semaine
- Fédération Bancaire Française (2025) : 53% des enfants 8-14 ans · moyenne 35€/mois
- Baromètre Pixpay (2025) : 26€/mois pour les ados français
- NatWest Rooster Money (2025) : UK · majorité des enfants · ~£10/semaine
- N26 / étude allemande : 71–95% des enfants selon l'âge reçoivent de l'argent de poche

---

## NOTES — PNGs À METTRE À JOUR

> Les schémas visuels existants sont partiellement obsolètes. Points à réviser :

**`1. architecture_fonctionnelle.png`**
- Ajouter : NWC endpoint par wallet enfant
- Ajouter : distinction Pool App vs Pot Parent
- Ajouter : Liquid multi-outputs (missions collectives)
- Mettre à jour : 4 types de missions (📋 ✏️ 🌐 ⭐)

**`2. flux_alimentation_bip85.png`**
- Inchangé — toujours valide

**`3. flux_reward_enfant.png`**
- Toujours valide pour le flux individuel
- Ajouter un schéma séparé pour le flux collectif (Liquid multi-outputs)

---

*"Can't be printed. Only earned."*

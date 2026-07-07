# UNPRINTED — Spécifications produit complètes
> Version : 0.2 — Hackathon  
> Dernière mise à jour : 2026-06-27  
> Usage : base de travail pour branding, pitch et développement

---

## 1. Vision produit

### Concept en une phrase
> Une app familiale Bitcoin qui éduque les enfants à la vraie valeur de l'argent — souveraineté financière, effort, risque — via des récompenses en sats gagnées, jamais offertes.

### Le problème
Les enfants grandissent avec une vision abstraite de l'argent : carte sans contact, virement invisible, argent de poche automatique. Résultat :
- Aucune notion d'effort ↔ récompense
- Aucune compréhension du risque, de l'échec, de la rareté
- Aucune souveraineté : tout passe par des intermédiaires

Les solutions existantes (Greenlight, Pixpay, tirelires connectées) reproduisent le même modèle fiat : custody bancaire, pas d'éducation réelle.

### Notre réponse
Un wallet Bitcoin self-custody pour toute la famille. L'enfant gagne des sats en accomplissant des défis — tâches, missions, quiz, projets entrepreneuriaux. Les parents fixent les règles, valident, et alimentent le pot. **Chaque satoshi a été mérité.**

### Ce qui nous différencie

| Critère | Nous | BTCBitByBit | Greenlight / Pixpay |
|---|---|---|---|
| Self-custody | ✅ dès le jour 1 (BIP-85) | ❌ custody | ❌ custody bancaire |
| Business model | % sur dépôts parents | Abonnement | Abonnement |
| Éducation | Bitcoin + finance + entrepreneuriat + corps + pensée critique | Bitcoin only | Gestion argent de poche |
| MiCA compliance | ✅ non-custodial exempt | ❓ | ✅ (banque partenaire) |
| Multi-enfants | ✅ BIP-85 (1 seed) | ❓ | ✅ |
| Lightning natif | ✅ Breez SDK | ❓ | ❌ |

---

## 2. Architecture technique

### Stack

```
App Mobile — React Native (iOS + Android)
  Interface Parent | Interface Enfant | Dashboard famille | Modules apprentissage | Backup seed guide

Logique métier (on-device + backend léger)
  Dérivation BIP-85 | Auto-sign rules | Verrou paiement | Push notifs | LNURL fee split

Breez SDK — Nodeless (Liquid) · open-source · Rust · gratuit
  Wallet self-custody | BOLT11/12 · LNURL | Submarine swaps | Magic Routing Hints | Clés sur device

Partenaires infrastructure
  Boltz (atomic swaps LN/L1/Liquid) | Transak (fiat onramp CB → sats) | FCM/APNs (push) | BIP-39/85 librairies Rust

Réseaux Bitcoin
  ⚡ Lightning Network | 🌊 Liquid Network | ₿ Bitcoin L1 | Non-custodial · hors périmètre MiCA
```

### Architecture familiale

```
Parent A (Wallet Parent)          Parent B
master seed BIP-39                même seed · lecture + validation
pot : X sats                      peut approuver rewards

        ↓ BIP-85 dérivation (1 seed parent → N seeds enfants)

Enfant 1          Enfant 2          Enfant 3          + enfant N
seed index 0      seed index 1      seed index 2      seed index N
wallet propre     wallet propre     wallet propre     wallet propre
```

### Flux d'alimentation du pot parent

```
CB (fiat via Transak)   ──┐
BTC Lightning externe   ──┤→ LNURL split ──→ Wallet Parent ──→ BIP-85 ──→ Wallets enfants
BTC on-chain (Boltz)    ──┘  fee ~1-2%       master BIP-39            dérivation
Contributeur tiers      ──┘  non-custodial   self-custody             automatique
```

La fee est prélevée par le protocole LNURL **avant** que les sats arrivent dans le wallet parent — l'app ne touche jamais les fonds des utilisateurs.

### Flux reward

```
1. Enfant        2. App enfant      3. Backend         4. Parent          5. Breez SDK       6. Enfant
Complète tâche → Génère invoice  → Reçoit invoice   → Valide (1 tap)  → Signe tx depuis → Reçoit X sats
ou QCM           LN · X sats       Notifie 2 parents   ou auto-sign       wallet parent      wallet propre
marque "fait"    via Breez SDK     push notification   si ≤ plafond       clé locale         ~26 sats fee
```

> Si parent et enfant utilisent tous les deux l'app : Magic Routing Hints → transaction Liquid directe (~26 sats, pas de swap Lightning)

### Cadre légal

**Non-custodial = hors périmètre MiCA CASP.**  
MiCA Article 3(1)(15) : un CASP est un prestataire qui *détient* des crypto-actifs. Notre app ne détient rien — les clés restent sur les devices.

- Pas de licence CASP requise
- Pas de KYC obligatoire (délégué à Transak pour la partie fiat)
- Structure légale légère : SAS française ou BV néerlandaise
- RGPD : données mineurs → consentement parental requis, zéro donnée personnelle dans le leaderboard (pseudonyme uniquement)

---

## 3. Catégories et objectifs

### Philosophie
L'app est organisée autour de **4 grandes catégories = 4 questions qu'un enfant peut se poser sur lui-même**. Approche non-scolaire, orientée compétences de vie.

La majorité des objectifs se réalisent dans la vie réelle avec validation parentale. Les missions app sont vérifiables numériquement.

---

### Catégorie 1 — 💪 Je prends soin de moi
*Corps, santé, hygiène, énergie*

| Sous-catégorie | Exemples d'objectifs | Vérification | Où |
|---|---|---|---|
| Sport & mouvement | 30 min de sport 3x/semaine, apprendre un sport | Parent | Vie réelle |
| Alimentation | 7 jours sans sucre ajouté, cuisiner un repas complet | Parent + photo | Vie réelle |
| Hygiène & routine | Se coucher avant 21h 5 jours, tenir une routine matinale | Parent | Vie réelle |
| Sommeil | Pas d'écran 1h avant le lit pendant 1 semaine | Parent | Vie réelle |
| Douche froide | 30 sec d'eau froide à la fin de la douche | Parent | Vie réelle |

---

### Catégorie 2 — 🧠 Je pense par moi-même
*Liberté, vie privée, scepticisme, expression*

| Sous-catégorie | Exemples d'objectifs | Vérification | Où |
|---|---|---|---|
| Vie privée & données | Expliquer ce qu'est un cookie, ne pas partager son adresse en ligne | Quiz app | App + Internet |
| Liberté d'expression | Écrire une opinion et la défendre, débattre à table | Parent | Vie réelle |
| Esprit critique | Vérifier une info avant de la croire, identifier une pub cachée | Parent + quiz | Internet + vie réelle |
| Histoire & argent | Expliquer pourquoi l'or a été confisqué en 1933, ce qu'est l'inflation | Quiz app | App |
| Médias | Comparer 2 sources différentes sur le même sujet | Parent | Internet |

---

### Catégorie 3 — ₿ Je comprends et gère mon argent
*Bitcoin, épargne, valeur du travail, préférence temporelle*

| Sous-catégorie | Exemples d'objectifs | Vérification | Où |
|---|---|---|---|
| Bitcoin & monnaie | Expliquer les 21M de BTC, la preuve de travail | Quiz app | App |
| Wallet & self-custody | Installer un wallet Lightning de test, envoyer 100 sats | Vérification on-chain | App + Internet |
| Épargne & patience | Mettre de côté 20% de ses sats pendant 4 semaines | App (automatique) | App |
| Mécanismes financiers | Comprendre l'intérêt composé, simuler un prêt | Quiz + simulateur app | App |
| Histoire monétaire | Comprendre l'étalon-or, la création monétaire, le halving | Quiz app | App |

---

### Catégorie 4 — 🛠️ Je crée de la valeur
*Entrepreneuriat, compétences pratiques, responsabilité, famille*

| Sous-catégorie | Exemples d'objectifs | Vérification | Où |
|---|---|---|---|
| Services & travail | Rendre service à un voisin, aider sans être demandé | Parent | Vie réelle |
| Entrepreneuriat | Vendre quelque chose, générer sa première facture | Parent + app | Vie réelle + app |
| Responsabilité | Tenir ses engagements 5 jours consécutifs | App (streak) | App + vie réelle |
| Famille & communauté | Expliquer Bitcoin à un adulte, aider un plus jeune | Parent | Vie réelle |
| Autosuffisance | Cultiver une plante, réparer un objet, cuisiner | Parent + photo | Vie réelle |

---

### Taxonomie complète des missions

| Label | Émetteur | Visible par | Sats | Points | Financement | Vérification |
|---|---|---|---|---|---|---|
| 📋 **Catalogue famille** | App (équipe/IA) | Tous les parents | ✅ | ❌ | Pot parent | Parent |
| ✏️ **Custom parent** | Parent | Famille uniquement | ✅ | ❌ | Pot parent | Parent |
| 🌐 **Mission ouverte** | App | Tous les enfants (selon âge/niveau) | ✅ | ✅ | Pool app | Numérique |
| ⭐ **Mission exclusive** | App | Top 3 hebdo uniquement | ✅ | ✅ | Pool app | Numérique |

**Règle clé :** les parents ne génèrent jamais de points — uniquement des sats. Ils peuvent augmenter le montant en sats d'une tâche difficile, mais pas influencer le classement.

#### Catalogue famille (📋)

```
Prédéfinies par l'équipe / enrichi par IA
Par thématique (4 catégories × sous-catégories)
× Par tranche d'âge : 3-6 / 7-11 / 12-15 / 16-18
× Par niveau de difficulté : ⭐ / ⭐⭐ / ⭐⭐⭐

Sources d'enrichissement du catalogue :
→ Core : équipe + IA entraînée sur corpus Bitcoin/éducation/pédagogie
→ Inspiration parents (opt-in) : parent coche "suggérer au catalogue"
  → mission anonymisée envoyée → review équipe → publication si validée
→ Stats agrégées (privacy-safe) : catégories populaires de missions custom
  remontent sans contenu — informent les priorités éditoriales
→ L'IA propose des variantes selon la progression individuelle de l'enfant
```

**Privacy — missions custom parent :** le contenu des missions custom ne remonte jamais automatiquement (risque de données perso : noms, lieux, contexte familial). Seule l'option opt-in explicite permet une suggestion anonymisée, soumise à review humaine avant toute intégration.

#### Missions ouvertes et exclusives (🌐 ⭐)

```
Vérification numérique uniquement (pas de validation parent) :
→ QCM / quiz interactif
→ Création de wallet guidée avec vérification on-chain
→ Screenshot avec validation algo
→ Géolocalisation (missions communautaires futures)

Déblocage :
→ Missions ouvertes 🌐 : progression personnelle (âge + niveau atteint par catégorie)
→ Missions exclusives ⭐ : top 3 du classement hebdomadaire uniquement
```

---

## 4. Features détaillées

### 4.1 Onboarding parent (5 min)

1. App génère une seed BIP-39 (12 mots)
2. Guidage backup physique obligatoire (confirmation)
3. Création profils enfants : app dérive seed BIP-85 par index
4. Définition des règles : plafond auto-sign, modules actifs, mode de jeu
5. Premier dépôt guidé (CB via Transak ou Lightning externe)

---

### 4.2 Interface parent

**Tableau de bord famille**
- Vue globale : pot parent + soldes enfants
- Notifications : demandes de validation en attente
- Historique complet par enfant

**Gestion des règles**
- Plafond auto-sign (en sats) : en-dessous → paiement automatique
- Modules activés par enfant
- Mode de jeu (Normal / Tricky / Investisseur)
- Définition des défis familiaux personnalisés

**Validation**
- Notification push (Parent A + Parent B, même seed, 2 téléphones)
- Un seul parent valide (verrou backend anti-double-paiement)
- Auto-sign si ≤ plafond défini

---

### 4.3 Interface enfant (adaptée par tranche d'âge)

**Écran principal**
- Solde en sats (dominant) + équivalent € spot (secondaire)
- Défis en cours avec progression et récompense associée
- Bouton "J'ai réussi" → déclenche la demande de paiement

**Historique**
- Liste chronologique des rewards : X sats = Y€ au moment du gain
- Dépenses (Bitrefill, rachat parent) dans le même historique
- Achievements (tâches complétées, badges)

---

### 4.4 Objectifs — quatre types (hors MVP)

**📋 Catalogue famille** — prédéfini par l'app
- Bibliothèque classée par catégorie / sous-catégorie / tranche d'âge / niveau
- Chaque mission contient :
  - Description enfant (simple, visuelle)
  - Description parent (contexte pédagogique)
  - Mode de vérification : parent
  - Type : unique ou récurrent (quotidien / hebdo / date fixe)
  - Récompense suggérée en sats (modifiable par le parent)
- Financé sur le pot parent · 0 point

**✏️ Custom parent** — ajouté librement par le parent
- Parent crée : titre, description, récompense en sats, délai
- Enfant peut proposer → validation parent requise
- Option opt-in "suggérer au catalogue" : mission anonymisée → review équipe
- Visible famille uniquement · financé sur le pot parent · 0 point

**🌐 Mission ouverte** — proposée par l'app
- Débloquée selon progression personnelle de l'enfant (âge + niveau par catégorie)
- Vérification numérique uniquement (quiz, wallet, screenshot)
- Financée sur le pool app · génère des points classement

**⭐ Mission exclusive** — proposée par l'app au top 3 hebdo
- Débloquée uniquement si l'enfant est dans le top 3 du classement hebdomadaire
- Plus difficile, mieux récompensée que les missions ouvertes
- Financée sur le pool app · génère des points classement

---

### 4.5 Planning & rappels

- Vue calendrier hebdomadaire avec tâches assignées
- Glisser pour déplacer une tâche dans le temps
- Rappels push configurables (enfant + parent)
- Notification parent pour validation en attente

---

### 4.6 Matrice Eisenhower + Backlog + Achievements

**Matrice Eisenhower (pour l'enfant)**
```
                 URGENT          PAS URGENT
IMPORTANT    [À faire maintenant] [Planifier]
PAS IMPORTANT   [Déléguer]        [Éliminer]
```

- Backlog visible en sidebar : tâches à venir non planifiées
- Enfant classe ses tâches dans les quadrants
- Tâches complétées → disparaissent du board → Achievements

**Achievements**
- Liste consultable : tâche, date, sats gagnés, catégorie
- Badges visuels par milestone (première tâche, 10 tâches, 1 000 sats, etc.)

---

### 4.7 Balance & comptabilité sats

**MVP**
- Solde : total sats + équivalent € au cours spot du jour
- Chaque reward affiche : "X sats = Y€ aujourd'hui"
- Historique : liste des rewards (sats + € au moment du gain) + dépenses

**Produit final**
- Graphique "valeur de mon épargne" : valeur en € du stack sur le temps
- Comparatif sound money : "Si tu avais gardé en €, tu aurais X€. En sats, tu as Y€."
- Affiché avec contexte éducatif (pas de regret, lecture de la volatilité comme feature)

---

### 4.8 Modes de récompenses (optionnel — choix parent)

Activable comme un mode de jeu. 3 niveaux :

**Normal**
Récompense versée immédiatement à la validation parent.

**Tricky** (simule l'incertitude de la vie)

| Événement | Probabilité | Effet |
|---|---|---|
| Paiement immédiat normal | 60% | Versement dès validation |
| Paiement retardé 3-7j | 15% | Auto-versement différé |
| Paiement oublié | 10% | Parent doit re-valider manuellement |
| Bonus surprise ×2 | 10% | Double récompense immédiate |
| Paiement non versé | 5% | Perte sèche · discussion parent/enfant |

> Le mode Tricky s'applique **uniquement aux récompenses app** (pool app), jamais aux récompenses parentales.

**Investisseur** (mécaniques financières)
- Blocage des sats X jours → taux défini à l'avance, non rachetable avant terme (simule un bond)
- Sortie libre → taux quotidien faible (simule un livret)
- Simulateur de gain affiché avant le choix

---

### 4.9 Mode Entreprise (optionnel — activable par le parent)

Objectif pédagogique : comprendre la création de valeur, les charges, le profit, le crédit.

#### Flux complet

**1. DEVIS**
- Enfant crée un service : nom, description, prix en sats
- App génère un devis PDF stylisé (template)
- Envoi au parent via notification push

**2. CONTRAT**
- Parent accepte → devient contrat horodaté
- Les deux parties valident (tap de confirmation)
- Délai de livraison défini dans le contrat

**3. EXÉCUTION + CHARGES**
- Enfant déclare ses charges (matériel, temps, outils) avec justificatif optionnel
- App calcule la marge prévisionnelle en temps réel
- Tableau de bord "mini-entreprise" : revenus prévus / charges / marge

**4. FACTURE**
- Enfant déclare service rendu → génère facture
- Parent valide la prestation (même flux que reward classique)
- Paiement via invoice Lightning depuis pot parent

**5. BONUS**
- Si marge > 0 (revenus > charges déclarées) :
  → Bonus = X% de la marge nette (% défini par le parent)
  → Versé automatiquement avec la facture

**6. CRÉDIT / LEVÉE DE FONDS**
- Enfant demande une avance pour couvrir ses charges
- Parent fixe : montant, taux d'intérêt (0–20%), délai de remboursement
- App génère un contrat de prêt horodaté
- Remboursement : déduit automatiquement sur chaque future facture (% configurable)

**7. DÉFAUT DE REMBOURSEMENT**
Si délai dépassé et solde insuffisant :
- Option A : -25% sur tous les futurs rewards jusqu'à remboursement complet
- Option B : Restructuration — parent renégocie (délai, montant réduit)
- Option C : Remise de dette — parent pardonne (enseignement sur la générosité)

Le statut "en défaut" est affiché dans l'interface enfant de façon pédagogique, pas punitive.  
Message : *"Tu dois X sats à tes parents. Voici comment les rembourser."*

---

### 4.10 Dépenser ses sats

**MVP**

| Solution | Description | Friction |
|---|---|---|
| Rachat parent | Bouton "Demander un rachat" → parent rachète les sats au cours spot et vire en fiat | Très faible |
| Bitrefill | Gift cards (Amazon, Steam, Netflix, Zalando…) payables en Lightning via API | Faible |

**Phase 2**
- Carte prépayée Visa rechargeable en sats (Bitcard / Strike Card)
- Annuaire commerçants BTC (BTCMap intégré)

> Note pédagogique : la rareté des points de dépense enseigne naturellement la valeur de l'épargne. L'enfant apprend que Bitcoin n'est pas "dépenser" mais "valoriser".

---

### 4.11 Classement & compétition (Phase 2 — opt-in parent)

#### Règle fondamentale
La compétition porte **uniquement** sur les missions app (jamais les tâches familiales). Les tâches familiales restent dans le référentiel personnel.

#### Profil compétitif
- Avatar + surnom pseudonyme choisis par l'enfant
- Zéro donnée personnelle exposée

#### Deux classements

| Classement | KPI | Reset |
|---|---|---|
| **Hebdomadaire** | Points missions app cette semaine | Lundi 00h00 |
| **All-time** | Points missions app cumulés depuis install | Jamais |

#### Architecture des missions (anti-circularité)

```
Progression personnelle (âge + missions complétées par catégorie)
        ↓
  Débloque des 🌐 Missions ouvertes (toujours disponibles)
        ↓
  L'enfant les complète → génère des POINTS
        ↓
  Points → Classement hebdo
        ↓
  Top 3 → accès aux ⭐ Missions exclusives (bonus supplémentaires)
```

**Missions ouvertes 🌐** — porte d'entrée du classement
- Débloquées par la progression personnelle uniquement (pas par le classement)
- Accessibles à tous les enfants selon leur âge et niveau atteint par catégorie
- Source principale de points

**Missions exclusives ⭐** — bonus du top 3
- Proposées en bonus chaque lundi après le reset, aux 3 meilleurs de la semaine précédente
- Génèrent aussi des points, mais ne sont pas nécessaires pour accéder au classement
- Un enfant démarre sans elles et monte grâce aux missions ouvertes

> Pas de dépendance circulaire : les missions ouvertes sont toujours disponibles indépendamment du classement.

#### Sources de points — règles

| Type | Label | Points | Sats | Financement |
|---|---|---|---|---|
| Mission ouverte | 🌐 | ✅ oui | ✅ oui | Pool app |
| Mission exclusive | ⭐ | ✅ oui | ✅ oui | Pool app |
| Catalogue famille | 📋 | ❌ non | ✅ oui | Pot parent |
| Custom parent | ✏️ | ❌ non | ✅ oui (montant libre) | Pot parent |

Les parents ne peuvent pas générer de points — uniquement ajuster les sats. Évite tout abus.

#### Calcul des points

```
Mission ⭐ débutant       = 10 pts
Mission ⭐⭐ intermédiaire = 25 pts
Mission ⭐⭐⭐ avancé      = 50 pts

Multiplicateur streak     : +10% par jour consécutif avec ≥1 mission complétée
Bonus diversité           : +15% si missions sur 3+ catégories différentes dans la semaine
```

#### Mécanique "marché des opportunités" (top 3 hebdo)

```
Reset hebdomadaire → classement figé → top 3 identifié

Top 3 du classement hebdomadaire reçoit une notification :
  → L'app génère un lot de N missions exclusives (difficiles, mieux récompensées)
  → Top 1 choisit en premier
  → Top 2 choisit parmi les restantes
  → Top 3 choisit parmi les restantes
  → Missions non choisies descendent aux rangs suivants (4e, 5e…)

Délai de choix : 12h — si dépassé, slot passe au rang suivant

Message pédagogique affiché :
"Ta progression t'a ouvert des portes que les autres n'ont pas.
D'autres enfants voulaient les mêmes missions — tu as choisi en premier."
```

Objectif : enseigner que la performance rend attractif aux yeux du marché et ouvre des opportunités.

---

### 4.12 Nostr Wallet Connect — plateforme ouverte (Phase 1)

L'app expose un endpoint NWC par wallet enfant. N'importe quel quiz Bitcoin, jeu éducatif, ou partenaire externe peut envoyer des sats à un enfant sans intégration spécifique côté app.

**Ce que ça change :** l'app passe de produit fermé à plateforme ouverte. Tout l'écosystème NWC-compatible (Alby, Zeus, partenaires éducatifs) devient un canal de récompense supplémentaire pour les enfants.

#### Fonctionnement

```
App génère une connexion NWC par wallet enfant
  → Chaîne de connexion (nostr+walletconnect://...) partagée par QR ou lien
  → Partenaire externe scanne / reçoit la chaîne
  → Partenaire peut envoyer des sats directement au wallet enfant via NWC
  → Plafond configurable par le parent (montant max par tx NWC)
  → Toute tx NWC apparaît dans l'historique enfant avec la source
```

#### Contrôle parental
- Parent active / désactive NWC par enfant
- Plafond par transaction (ex : max 1 000 sats par tx NWC)
- Plafond journalier optionnel
- Notification push parent à chaque réception NWC

#### Cas d'usage immédiats
- Quiz Bitcoin externe (Bitcoinero, jeux éducatifs) → récompense directe dans le wallet
- Grands-parents / proches envoient des sats sans créer de compte
- Partenaires hackathon / écosystème peuvent démontrer l'intégration en live

#### Stack
- Breez SDK supporte NWC nativement (Nodeless)
- Backend : stocker les connexions NWC actives par enfant, appliquer les plafonds
- Effort : moyen · Impact écosystème : maximal

---

### 4.13 Missions collectives (Phase 1 — même famille uniquement)

#### Périmètre retenu

- Enfants de la même famille uniquement (tous sur l'app, wallets Liquid BIP-85 dérivés)
- Pas d'enfant externe pour l'instant
- **Mode collaboratif uniquement** — tous les enfants doivent compléter leur partie avant paiement
- Mode parallèle : version ultérieure
- Mode compétitif : abandonné

#### Mécanisme technique

```
1 tx Liquid multi-outputs (atomique)
  → Parent wallet signe une transaction Liquid avec N outputs
  → Chaque output = wallet Liquid de l'enfant concerné
  → Soit tous reçoivent, soit personne (atomicité garantie)
  → Fee unique pour l'ensemble (~30-60 sats) vs ~26 sats × N en séquentiel
  → Natif Breez SDK Nodeless — aucune infrastructure supplémentaire
```

#### Flux complet

**Parent — création**
1. Nouvelle mission → "Mission collective"
2. Sélectionner les enfants participants (avatars à cocher)
3. Description de la tâche + répartition des rôles par enfant
4. Récompenses : égales pour tous OU personnalisées par enfant
5. Délai de complétion + option "bonus de groupe" (X% si tous terminent avant la date)
6. Option "déblocage partiel" : payer les complétés même si groupe incomplet (oui/non)
7. Confirmer → mission apparaît dans l'app de chaque enfant

**Enfant — vue mission collective**
```
Bandeau "Mission de groupe 👥"
Avatars des participants + statut de chacun (En cours / Fait / Validé)
Progression : "2 / 3 parties validées"
Bouton "J'ai réussi" → soumet sa partie pour validation parent
```

**Parent — validation**
```
Notification par enfant dès qu'il soumet sa partie
Parent valide chaque contribution individuellement (photo, commentaire)
Quand TOUS validés → bouton "Payer le groupe"
  → 1 tap → 1 tx Liquid multi-outputs → chaque enfant reçoit simultanément
Si "déblocage partiel" activé → "Payer les X enfants complétés" disponible avant
```

#### Gestion des cas limites

| Cas | Comportement |
|---|---|
| Un enfant ne termine pas avant le délai | Parent peut prolonger, débloquer partiellement, ou annuler |
| Parent veut payer différemment de ce qui était prévu | Modification possible avant paiement |
| Enfant veut quitter le groupe | Parent retire l'enfant → mission se recalcule |
| Désaccord entre enfants sur les rôles | Parent arbitre via commentaire · enseigne la négociation |

#### Dimension pédagogique

- Coordination : le groupe avance à la vitesse du plus lent
- Responsabilité envers les autres, pas seulement envers soi
- Négociation et répartition du travail
- Bonus de groupe : la valeur collective peut dépasser la somme des valeurs individuelles

---

## 5. Modèle économique

**Aucun abonnement.** Les parents paient uniquement quand ils alimentent le pot.

| Source de revenus | Mécanisme | Estimation |
|---|---|---|
| Fee sur dépôt parent | LNURL-pay split (prélevé avant réception, non-custodial) | 1–2% |
| Revenue share Transak | Partenariat fiat onramp | ~0.3–0.5% du volume CB |

**La pool app sert à :**
1. Financer les coûts business (dev, marketing, légal, infra)
2. Rémunérer les missions app débloquées par la progression de l'enfant (hors cercle familial)

**Exemple :** 1 000 familles × 50€/mois × 1.5% = **750€/mois dès 1 000 familles**

Croissance naturelle : plus les enfants progressent → parents rechargent plus → revenus croissent avec l'engagement.

---

## 6. Roadmap

### Phase 0 — MVP Hackathon (48h)
- [ ] Wallet parent (BIP-39 + Breez SDK)
- [ ] 1 profil enfant (BIP-85 index 0)
- [ ] 1 défi prédéfini + bouton "J'ai réussi"
- [ ] Flux reward : invoice LN → validation parent → versement
- [ ] Auto-sign si ≤ plafond défini
- [ ] Affichage solde sats + équivalent €

### Phase 1 — Beta (M1–M3)
- [ ] Multi-enfants (BIP-85 multi-index)
- [ ] Catalogue missions (toutes catégories, 3 tranches d'âge)
- [ ] Missions collectives — mode collaboratif (Liquid multi-outputs, même famille)
- [ ] NWC endpoint par wallet enfant (plafond parental, historique)
- [ ] Matrice Eisenhower + backlog + achievements
- [ ] Planning + rappels push
- [ ] Transak fiat onramp intégré
- [ ] Bitrefill intégré
- [ ] Mode Tricky
- [ ] Balance historique + comparatif sound money (MVP)

### Phase 2 — Launch (M4–M6)
- [ ] App stores (iOS + Android)
- [ ] Compétition opt-in (leaderboard + mécanique marché des opportunités)
- [ ] Mode Entreprise (devis, contrat, facture, crédit)
- [ ] Mode Investisseur (blocage sats / taux)
- [ ] Comparatif sound money avancé
- [ ] 100 familles beta, partenariats écoles/communautés Bitcoin

---

## 7. Epics & périmètre MVP

> Lecture : **P0** = MVP Hackathon · **P1** = Beta M1–M3 · **P2** = Launch M4–M6

| Epic | Priorité | Stack |
|---|---|---|
| Wallet parent (seed BIP-39, Breez SDK, dépôt Lightning/onchain) | **P0** | Breez SDK · BIP-39 · Boltz · Transak |
| Wallet enfant BIP-85 + flux reward (invoice → validation → paiement auto-sign) | **P0** | Breez SDK · BIP-85 · Backend (verrou) · FCM |
| Balance sats + équivalent € + historique rewards | **P0** | Breez SDK · API prix BTC |
| NWC endpoint par enfant (plafond parental, historique) | **P1** | Breez SDK NWC · Backend |
| Multi-enfants (N wallets BIP-85) + profils | **P1** | BIP-85 · React Native |
| Catalogue missions 4 types (📋 ✏️ 🌐 ⭐) + planning + rappels | **P1** | Backend · FCM · React Native |
| Missions collectives (Liquid multi-outputs, mode collaboratif) | **P1** | Breez SDK · Liquid · Backend |
| Onramp fiat (Transak) + dépenses (Bitrefill + rachat parent) | **P1** | Transak API · Bitrefill API |
| Matrice Eisenhower + backlog + achievements + milestones | **P1** | React Native · Backend |
| Mode Tricky (incertitude simulée) + Mode Investisseur (blocage/intérêt) | **P1** | Backend · React Native |
| Classement & compétition opt-in (leaderboard hebdo + all-time + marché des opportunités) | **P2** | Backend · React Native |
| Mode Entreprise (devis · contrat · facture · crédit · pénalités) | **P2** | Backend · React Native · PDF |
| Sound money avancé (graphique valeur sats vs €, comparatif historique) | **P2** | API prix BTC · React Native |

**Ligne de coupe MVP recommandée : après les 3 epics P0.**  
Le flux complet (parent → enfant → reward) est démontrable en 48h. NWC peut être ajouté comme demo écosystème si le temps le permet (Breez SDK l'expose nativement, peu d'effort marginal).

---

## 8. Questions ouvertes

1. **Nom de l'app** — à définir (branding)
2. **Tranche d'âge minimum** — 3-6 ans implique interface très visuelle, quasi-parent-driven
3. **Missions app** — contenu initial du catalogue à rédiger
4. **% bonus mode Entreprise** — valeur par défaut suggérée au parent ?
5. **Fee exacte** — 1% ou 2% ? À tester avec modèle financier
6. **Bitrefill API** — intégration native ou deeplink vers leur app ?
7. **Langue** — français uniquement au lancement ou multilingue dès le MVP ?
8. **Carte prépayée Phase 2** — partenaire à identifier (Bitcard, Strike, autre)

---

*"Donne un sat à un enfant, il mange un jour. Apprends-lui à en gagner, il comprend la valeur pour toujours."*

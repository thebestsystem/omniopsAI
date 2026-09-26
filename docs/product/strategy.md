# Strategy — OmniOps AI

> Document de stratégie & modèle économique. Cohérent avec le PRD v4 (`prd.md`).
> Statut : hypothèses de travail — à confronter aux 2–3 premiers design partners.

---

## 1. Thèse d'investissement

> Un retailer OneStock a un problème de décision, pas de données : sa politique de
> disponibilité est statique, non mesurée, et personne ne sait prouver qu'une
> alternative serait meilleure. OmniOps vend la **preuve** — un audit payé qui
> démontre une opportunité chiffrée, puis une plateforme qui transforme la décision
> en expérimentation contrôlée et mesurée.

Trois convictions structurantes :

1. **La valeur est dans la décision, pas dans l'algorithme.** Le client ne paie pas
   un score, il paie la certitude qu'une règle à modifier améliorera un KPI sans en
   casser un autre.
2. **L'audit est l'acquisition, pas un service accessoire.** Chaque audit payé doit
   soit convertir en design partner, soit produire une preuve réutilisable.
3. **Le SaaS se mérite.** On ne construit la plateforme qu'après la preuve produit
   (règle d'or du PRD). Tout investissement avant cette preuve est de la dette.

---

## 2. Modèle économique

### 2.1 Échelle de valeur (du service vers le logiciel)

```text
Étage 0 — Decision Audit (service productisé)
  Prix : €5k–15k · 4–8 semaines · marge faible, objectif = acquisition + preuve
        ↓ convertit en
Étage 1 — Design Partner (audit + expérimentation contrôlée)
  Prix : €10k–25k · le client lance un test live · 1ère preuve de réalité
        ↓ convertit en
Étage 2 — SaaS Experiment
  Prix : €30k–50k ARR · backtest self-service, comparaison de politiques, test
        ↓ étend en
Étage 3 — SaaS Decision
  Prix : €50k–80k ARR · Decision Ledger, mesure d'impact, historique
```

Le SaaS ne démarre que si au moins un client a **lancé un test** et qu'un résultat
réel a été mesuré (cf. PRD §17).

### 2.2 Unit economics (hypothèses de travail, à valider)

**Étage 0 — Audit (le point de fragilité)**

| Poste | Hypothèse |
|---|---|
| Prix moyen | €10k |
| Durée | ~6 semaines |
| Effort fondateur | ~25–30 j de travail |
| Marge brute | ~0 (effort ≈ prix) — **assumé, c'est le coût d'acquisition** |
| Objectif réel | 1 preuve + 1 converti, pas de la marge |

**Étage 2/3 — SaaS (où se capte la valeur)**

| Poste | Hypothèse |
|---|---|
| ARR design partner → SaaS | €30–50k |
| Marge brute | 85–90 % (ingestion batch, coût marginal faible) |
| Churn cible (bien géré) | < 10 % / an |
| LTV brute | ~€150–250k (ARR × 3–5 ans conservateur) |
| CAC payé (audit) | ~€10k en temps fondateur |
| Ratio LTV/CAC | > 15 (faussé : le CAC est du temps, pas du cash) |

**Lecture honnête :** le ratio LTV/CAC flatteur masque le vrai risque — le goulot
n'est pas le coût d'acquisition, c'est le **temps fondateur** et la **capacité à
répéter l'audit sans y passer 6 semaines à chaque fois**. La productisation de
l'audit (pipeline rejouable, cf. Phase 0) est l'enjeu économique n°1, devant le SaaS.

### 2.3 Règle de tarification

Le prix se cale sur l'**ordre annuel, le nombre de points de fulfillment, les marchés
et le nombre de politiques gérées** — jamais sur le nombre d'utilisateurs (cf. PRD §17).
Le pricing par utilisateur détruirait l'ancrage de valeur (le décideur paie pour un
impact opérationnel, pas pour des sièges).

---

## 3. Marché

### 3.1 Données d'ancrage OneStock (source publique, 2024–2025)

- Fondé 2010 (Toulouse) ; ~170 employés ; Series A **$72M** (mai 2024).
- **100+ retailers/brands internationaux**, **25 pays**, **15 000 stores**, **€2,5 Md
  d'ordres orchestrés/an**, NPS +63.
- Clients notables : LVMH, **Longchamp**, ManoMano, Intersport, Yves Rocher, WHSmith,
  Pets at Home, Hackett, The Perfume Shop, Dune, Moss, Maisons du Monde, Orlebar Brown,
  JD Sports.
- Positionnement recentré : « AI-driven Distributed Order Management » + « Promise
  Engine » + MCP server pour l'agentic commerce.
- **Écosystème partenaires structuré** : Solution Integrators (SIs), Independent
  Software Vendors (ISVs), Extensions Portal, membre MACH Alliance. Architecture
  extensible (GCP, Kubernetes, Golang/Node, MongoDB/Redis) avec un point clé :
  « core extensibility — influence promise and stock visibility **with external
  data** » + « AI agent extensibility ».

### 3.2 TAM / SAM / SOM (chiffres sourcés, hypothèses de périmètre marquées)

> Les chiffres de marché ci-dessous sont **sourcés** (IHL, NielsenIQ, communiqués
> OneStock). Les hypothèses de périmètre (ICP) restent à confirmer client par client.

**Valeur du problème (ancrage macro, à rapporter avec prudence au wedge « dernière unité ») :**
- **$984 Md** de ventes perdues/an liées aux ruptures de stock dans le monde, dont
  **$144,9 Md** en Amérique du Nord (IHL Group).
- **$82 Md** de ventes CPG perdues aux US en 2021, on-shelf availability **92,6 %**
  (NielsenIQ).
- $22 Md de ventes perdues en ligne liées aux out-of-stocks (GMA / Corsten & Gruen).

Ces chiffres concernent surtout le retail CPG/masse — ils ne quantifient pas le wedge
« dernière unité en OMS » — mais ils ancrent l'ordre de grandeur du coût d'une rupture
de disponibilité et servent de cadrage dans un deck (étiquetés « macro »).

**TAM / SAM / SOM :**
- **TAM (vision)** : le coût de la mauvaise décision de disponibilité chez tous les
  retailers omnicanaux multi-nodes, tous OMS confondus.
- **SAM (beachhead)** : retailers OneStock avec SFS/C&C significatif et ≥ 30 points de
  fulfillment (ICP, PRD §5). Borné sup : **~100+ comptes** OneStock (source officielle
  2024). Le filtre ICP ramène ce SAM à **quelques dizaines** de comptes réalistes.
- **SOM (réaliste 24 mois)** : 2–3 design partners payants, puis 5–10 comptes SaaS. Le
  marché servable à court terme se compte en **dizaines**, pas en milliers.

**Méthode de quantification restante avant pitch investisseur :**
1. filtrer les ~100 clients OneStock par l'ICP (SFS actif + ≥ 30 nodes + équipe OMS) ;
2. valeur annuelle moyenne (ancrage : €50k ARR × taux de pénétration) ;
3. construire le bottom-up SOM depuis les 2–3 premiers comptes réels.

### 3.3 Signaux favorables

- OneStock a levé $72M → budget R&D et marketing, mais aussi **pression pour monétiser**
  → un partenaire qui aide ses clients à extraire de la valeur est bienvenu (co-vente
  possible à moyen terme).
- La promesse OneStock (« Promise Engine », « ambitious promises ») crée la demande
  pour un **tiers qui mesure si la promesse est tenue** — c'est exactement la place
  d'OmniOps.

---

## 4. Concurrence & position défendable

### 4.1 Le beachhead est aussi la menace n°1

OneStock (et tout OMS « AI-driven ») peut absorber l'intelligence de disponibilité :
il détient les données temps réel, le moteur d'allocation et la relation client.

**Ce qu'OmniOps ne doit jamais construire comme cœur de valeur** (sera absorbé) :
- un dashboard de monitoring OMS ;
- un score de disponibilité exposé à côté du tableau de bord OneStock ;
- un « copilote » qui recommande sans preuve.

**Ce qu'OmniOps construit et qui résiste à l'absorption :**
1. **Indépendance** — l'audit d'une politique n'est crédible que si l'auditeur n'est
   pas le vendeur du moteur d'exécution. « Nous ne vendons pas l'OMS » est un atout
   de confiance.
2. **Backtest comparable** — rejouer plusieurs politiques sur la même période, la
   même population, avec incertitude affichée. OneStock n'a pas intérêt à montrer que
   sa config par défaut est sous-optimale.
3. **Expérimentation contrôlée** — A/B magasin, switchback, guardrails, mesure
   traitement/contrôle. C'est au-delà du périmètre OMS.
4. **Decision Ledger** — mémoire versionnée des politiques, hypothèses et résultats,
   réutilisable d'une période à l'autre.
5. **Multi-OMS à terme** — un benchmark qui devient crédible *parce qu'il* est
   indépendant de l'OMS, pas malgré.

### 4.2 Concurrence indirecte

- **Équipes data internes** : peuvent faire un one-off, pas un produit rejouable ni un
  benchmark sectoriel. → vendre la répétabilité et le coût marginal.
- **BI / SQL / warehouse** : décrivent, ne décident pas, ne backtestent pas.
- **Cabinets spécialisés** : font l'audit (concurrence à l'étage 0), pas le SaaS de
  suivi d'expérimentation. → les traiter comme canal, pas comme ennemi (un cabinet
  peut revendre l'audit OmniOps).

### 4.3 Moats

Le moat n'est pas technologique (un backtest se copie) — il est **composé** :
données opérationnelles normalisées + politiques versionnées + backtests
reproductibles + expérimentation contrôlée + mémoire des décisions (PRD §18). La
défense se construit client par client, en accumulant un **historique de décisions
documentées** qu'aucun concurrent ne peut rattraper rapidement.

---

## 5. Go-to-market

### 5.1 Séquence (beachhead OneStock, puis expansion)

```text
1. 2–3 design partners OneStock payants
   → preuve de réalité (Level 3 : amélioration mesurée)
        ↓
2. Productiser l'audit (pipeline rejouable, < 2 semaines d'effort)
   → rentabilité du temps fondateur
        ↓
3. SaaS Experiment sur ces comptes (backtest self-service)
        ↓
4. Élargir : plus de politiques (sourcing, capacité, split), puis multi-OMS
   (Fluent, Manhattan, SAP)
```

### 5.2 Motion d'entrée

L'offre d'entrée est un **audit payé**, pas une démo plateforme (PRD §17) :

> « Nous analysons 90 jours de données OneStock, reconstruisons votre politique
> actuelle, la comparons à des règles alternatives et identifions un test contrôlé
> sur un petit périmètre. »

La qualification du premier rendez-vous porte sur **5 portes** (PRD §5/§17) :
politique modifiable, problème économique, accès aux données, responsable identifié,
possibilité de groupe contrôle. Un prospect qui n'ouvre pas ces 5 portes n'est pas un
prospect — ne pas le poursuivre.

### 5.3 Canaux

- **Chaud (prioritaire)** : réseau intégrateurs OneStock, anciens collègues OMS,
  communautés retail omnicanal (France/EU d'abord, ancrage OneStock Toulouse).
- **Contenu de preuve** : publier un *anonymised audit* (une étude de cas sans nom de
  client) qui montre « politique A vs B sur la même période ». C'est le seul marketing
  qui crédibilise sans dévoiler un client.
- **Co-vente OneStock (moyen terme)** : uniquement après preuve, et en gardant
  l'indépendance comme argument — positionner OmniOps comme le tiers de mesure, pas
  un intégrateur. La voie ISV OneStock (Extensions Portal) est réelle mais doit
  rester une option de distribution, jamais une dépendance.

### 5.4 Le levier Longchamp — à activer avec précaution

**Longchamp est client OneStock** (source publique : communiqué OneStock, « optimise
CX et omnichannel sur 80 pays »). C'est le design partner le plus rapide et le plus
qualifié de l'ICP : fashion/luxury, SFS actif, multi-pays, ≥ 30 points de fulfillment.

Le fondateur (Belhaj) y intervient comme prestataire externe — ce qui donne un accès
unique au contexte métier et aux données OneStock.

**Deux conditions avant d'en faire un terrain de preuve :**
1. **Clarifier le cadre contractuel** — vérifier la clause de non-concurrence, la
   propriété des données et l'autorisation écrite d'utiliser des exports (même
   anonymisés) pour construire un produit tiers. Sans autorisation écrite : ne pas
   toucher aux données.
2. **Séparer les rôles** — en interne Longchamp on est prestataire ; pour OmniOps on
   est fondateur. Documenter par écrit qui fournit quoi, et obtenir un accord du
   décideur (Head of Omnichannel / DSI).

Si Longchamp ne peut pas servir de design partner (blocage contractuel), le plan B
est un client tiers de l'écosystème OneStock via un intégrateur (cf. §5.3).

---

## 6. Risques & mitigations

| Risque | Signal d'alerte | Mitigation |
|---|---|---|
| Heuristiques simples = modèle dynamique | Lift non significatif | PRD §21 : kill criteria — pivoter, ne pas forcer le modèle |
| `inventory_at_allocation` absent | Audit bloqué au L1 | Reconstruction par snapshots horodatés (spec extraction §D) |
| OneStock absorbe la fonction | Fonction « intelligence » intégrée à l'OMS | Ne pas vendre le score ; vendre indépendance + backtest + test contrôlé |
| Temps fondateur non rentable | Audit > 6 semaines récurrent | Productiser le pipeline Phase 0 en priorité |
| Aucun client n'accepte un test | Design partner signe sans tester | Ne pas construire le SaaS ; re-qualifier l'offre |
| Contamination du contrôle (réallocation) | OneStock réalloue entre groupes | Marquer l'expérimentation « contaminée », ne pas conclure (PRD §13) |

---

## 7. Soutenabilité & jalons

La stratégie est **auto-financée par l'audit** : chaque audit payé couvre ~son coût
et produit une preuve ou un client. Pas de besoin de capital externe avant la preuve
produit.

Jalons de validation :

1. **Preuve de données** — 2 clients fournissent les exports minimaux (L1/L2).
2. **Preuve de décision** — 1 politique alternative préférée en backtest.
3. **Preuve de réalité** — 1 test live, amélioration mesurée, guardrails respectés.
4. **Preuve commerciale** — 2 clients continuent après le pilote ; ARR crédible ≥ 3×
   le prix envisagé.
5. **Preuve de répétabilité** — un audit mené en < 2 semaines d'effort.

---

## 8. Décisions stratégiques à trancher

1. **Cible géographique du 1er design partner** — France/EU (proximité OneStock
   Toulouse, réseau personnel) vs UK/US (tickets plus élevés).
2. **Positionnement vs OneStock** — « tiers de mesure indépendant » (recommandé) vs
   « partenaire de co-vente » (prématuré avant preuve).
3. **Le prix plancher de l'audit** — €5k fixe productisé (plus rapide à vendre) vs
   €10–15k sur-mesure (plus de marge, plus long). Recommandation : démarrer à €5–8k
   productisé pour maximiser le nombre de preuves, monter après la 1ère preuve.
4. **Canal d'acquisition initial** — intégrateurs OneStock (rapide) vs contenu de
   preuve anonymisé (lent mais différenciant). Recommandation : les deux, intégrateurs
   d'abord.

> Prochaine étape recommandée : `roadmap.md` (jalons datés + go/no-go par phase),
> puis trancher la stack Phase 0.

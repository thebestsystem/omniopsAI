# PRD v4 — OmniOps AI

**Version :** 4.0  
**Statut :** Discovery / Design Partner  
**Catégorie :** B2B SaaS — Retail Operations / OMS Decision Support  
**Wedge initial :** retailers utilisant OneStock  
**Use case initial :** politique de disponibilité de la dernière unité  
**Mode initial :** audit batch read-only, puis expérimentation contrôlée  
**ICP initial :** retailers omnicanaux complexes avec Ship-from-Store et au moins 30 points de fulfillment  
**Positionnement :** Decision Audit & Policy Experimentation pour opérations omnicanales

---

# 1. Résumé exécutif

OmniOps aide un retailer à répondre à une question opérationnelle précise :

> **Quelle politique de disponibilité expose le plus de stock réellement vendable sans augmenter excessivement les échecs de fulfillment ?**

Le produit ne vend ni un score IA, ni un dashboard supplémentaire, ni un copilote OneStock.

Il vend une boucle de décision mesurable :

```text
Données → Baseline → Politiques candidates → Backtest → Test contrôlé → Résultat réel
```

OneStock reste le système opérationnel qui exécute les règles. OmniOps intervient avant et après la décision :

- reconstruire la performance de la politique actuelle ;
- comparer des règles simples et contextuelles ;
- expliciter les trade-offs ;
- proposer un test limité ;
- mesurer le résultat contre un contrôle ;
- conserver la décision et son résultat.

La première offre est un **Productized Decision Audit**. Le SaaS ne sera construit que si l’audit démontre une opportunité réelle, mesurable et payée.

---

# 2. Problème client

Les retailers disposent souvent de politiques statiques :

- buffer identique pour tous les magasins ;
- masquage uniforme de la dernière unité ;
- règles fondées sur l’expérience ;
- modifications après incident ;
- analyses dispersées entre OMS, SQL, BI et fichiers Excel.

Ces règles peuvent simultanément :

- exposer du stock qui échouera lors de la préparation ;
- masquer du stock vendable ;
- provoquer des réallocations ;
- augmenter les annulations ;
- détériorer la promesse client ;
- réduire la marge par commande.

Le problème n’est pas principalement de détecter une anomalie. Le problème est de savoir quelle politique choisir et comment prouver qu’elle est meilleure.

---

# 3. Positionnement

## Promesse commerciale

> **Testez votre politique de disponibilité avant de modifier OneStock, puis mesurez son effet réel sur les échecs de fulfillment et le stock vendable.**

## Ce que le produit n’est pas

OmniOps n’est pas :

- un remplacement de OneStock ;
- un outil de monitoring OMS ;
- une BI générique ;
- un chatbot ;
- un moteur d’alertes ;
- un outil de prédiction présenté sans décision associée ;
- un système d’écriture automatique dans le MVP ;
- un digital twin avant d’avoir démontré la capacité de replay.

## Frontière concurrentielle

OneStock peut fournir l’exécution, la visibilité et progressivement certaines fonctions d’intelligence. OmniOps doit rester différencié par :

- benchmark indépendant de politiques ;
- backtest comparable sur une même période ;
- expérimentation contrôlée ;
- mesure de l’écart entre estimation et réalité ;
- preuve économique ;
- historique réutilisable des décisions.

Toute fonctionnalité qui pourrait être facilement absorbée par l’OMS ne doit pas être le cœur de la proposition de valeur.

---

# 4. Wedge initial : dernière unité

Le premier use case est volontairement limité à une seule action :

> **Masquer ou exposer la dernière unité disponible selon le contexte de fulfillment.**

Exemples de politiques comparées :

```text
Policy A — Current
Buffer = 1 partout

Policy B — Simple
Masquer toute position avec inventory <= 1

Policy C — Store reliability
Masquer inventory = 1 lorsque la fiabilité du magasin < 92 %

Policy D — Contextuelle
Masquer inventory = 1 lorsque la fiabilité du magasin ou du SKU est sous un seuil
```

Pas de sourcing dynamique, de capacité magasin, de split shipment ou d’optimisation multi-objectifs dans le premier produit.

---

# 5. ICP et acheteurs

## ICP

Retailer avec :

- OneStock en production ;
- Ship-from-Store ou Click & Collect significatif ;
- au moins 30 magasins ou nodes de fulfillment ;
- stock faible sur certains SKU ;
- volume suffisant pour comparer des cohortes ;
- équipe OMS, omnichannel ou e-commerce dédiée ;
- capacité à modifier une règle sur un périmètre limité ;
- accès à au moins 60 jours de données.

Verticales prioritaires : fashion, luxury, footwear, sportswear, beauty et specialty retail.

## Utilisateur principal

OMS / Omnichannel Product Owner :

> “Je veux savoir quelle règle modifier et éviter de détériorer un autre KPI.”

## Acheteur économique

Head of Omnichannel ou E-commerce Director :

> “Je paie si l’impact sur la disponibilité, les annulations ou la marge est mesurable.”

## Gate organisationnel

Le prospect n’est qualifié que s’il peut nommer :

1. un propriétaire de la politique ;
2. une métrique primaire ;
3. une personne capable d’autoriser un test ;
4. un périmètre de traitement et de contrôle.

---

# 6. Hypothèse falsifiable

> **Une politique contextuelle de dernière unité peut améliorer le compromis entre disponibilité et fulfillment reliability par rapport à la politique actuelle et à des heuristiques simples.**

Cette hypothèse n’est pas considérée comme vraie avant trois preuves :

```text
Level 1 — Prediction
Le modèle bat une baseline simple.

Level 2 — Policy
La politique candidate améliore le trade-off en backtest.

Level 3 — Reality
Le test contrôlé produit une amélioration exploitable.
```

Une preuve au Level 1 ou 2 ne suffit pas à valider le produit.

---

# 7. North Star et métriques

## North Star

**Implemented Policy Impact** : valeur économique observée provenant d’une politique influencée par OmniOps.

## Métriques de validation

### Prédiction

- calibration ;
- top-decile lift ;
- precision / recall ;
- fulfillment failures évités par unité de disponibilité sacrifiée.

### Politique

- fulfillment failure rate ;
- disponibilité exposée ;
- positions masquées ;
- réallocations ;
- annulations ;
- shipping ou handling cost si disponible.

### Expérimentation

- différence traitement / contrôle ;
- intervalle d’incertitude ;
- respect des guardrails ;
- taille d’échantillon ;
- cohérence avec le backtest.

### Adoption

- audits payés ;
- politiques candidates testées ;
- tests live lancés ;
- recommandations conduisant à une modification réelle.

Une analyse seulement consultée n’est pas une adoption.

---

# 8. Limites de mesure et demande perdue

Le masquage de stock crée un problème de contrefactuel : les ventes non réalisées ne sont pas directement observables.

OmniOps doit donc distinguer quatre catégories :

### Observed

Mesuré directement : commandes, fulfillment, réallocations, annulations et stock disponible reçu.

### Estimated

Inféré par un modèle ou une comparaison statistique.

### Assumed

Hypothèse métier fournie ou validée par le client.

### Unknown

Non identifiable avec les données disponibles.

Le produit ne doit pas présenter une perte de conversion ou un GMV protégé comme une valeur observée si la demande cachée n’est pas mesurée.

## Data nécessaire pour estimer la demande perdue

Lorsque possible, intégrer :

- impressions de disponibilité ;
- recherches produit ;
- add-to-cart ;
- conversion ;
- sessions ;
- commandes abandonnées ;
- données de période comparable ;
- demande par canal.

Sans ces données, le rapport doit afficher une fourchette et une analyse de sensibilité, jamais un montant précis présenté comme certain.

---

# 9. Phase 0 — Productized Decision Audit

## Offre

Durée : **4 à 8 semaines**.  
Prix cible : **€5k à €15k**, selon périmètre et qualité des données.

Entrée :

- 60 à 180 jours de commandes ;
- lignes de commandes ;
- fulfillment location ;
- outcome ;
- inventory au moment de l’allocation si disponible ;
- réallocations et annulations ;
- magasins ;
- politique et buffers actuels ;
- métriques de conversion ou de demande si disponibles.

Sortie :

1. rapport de qualité et disponibilité des données ;
2. baseline de la politique actuelle ;
3. segmentation des échecs ;
4. deux ou trois politiques simples ;
5. modèle contextuel uniquement s’il bat les baselines ;
6. comparaison des trade-offs ;
7. estimation économique avec incertitude ;
8. plan de test contrôlé ;
9. décision go / no-go du client.

## Data Readiness Levels

### Level 1

Orders + outcomes : segmentation de fiabilité et baseline d’échec.

### Level 2

+ inventory au moment de l’allocation : analyse dernière unité et backtest limité.

### Level 3

+ configuration et buffers versionnés : reconstruction plus fiable de la politique.

### Level 4

+ POS, WMS, ajustements, coûts et conversion : analyse économique et demande perdue plus robuste.

Le produit peut démarrer au Level 1, mais aucune promesse de politique de stock ne doit être faite avant le Level 2.

## Gate de validation

Ne pas construire le SaaS si l’un des éléments suivants est absent :

- données techniquement accessibles ;
- baseline fiable ;
- politique alternative meilleure que les heuristiques simples ;
- client prêt à tester ;
- responsable identifié ;
- paiement du diagnostic ou engagement contractuel ;
- métrique économique suffisamment crédible.

---

# 10. Baselines obligatoires

Chaque audit compare au minimum :

```text
Baseline A
Masquer toute position avec inventory <= 1

Baseline B
Buffer = 1 partout

Baseline C
Buffer = 1 pour les magasins dépassant un seuil d’échec

Baseline D
Politique actuellement utilisée par le retailer
```

Un modèle dynamique n’est retenu que s’il produit un lift exploitable et stable sur une période hors entraînement.

Le modèle préféré est le plus simple qui bat les baselines :

1. taux empiriques ;
2. lissage bayésien ;
3. régression logistique ;
4. gradient boosting uniquement si nécessaire.

---

# 11. MVP — OmniOps Experiment

Le MVP doit permettre à un OMS Product Owner de comparer une politique actuelle à plusieurs alternatives et de concevoir un test limité.

## P0

- import batch API, export, SFTP ou warehouse ;
- modèle normalisé ;
- data quality report ;
- représentation versionnée de la politique ;
- baselines ;
- modèle de fulfillment success probability ;
- backtest historique ;
- comparaison de politiques ;
- analyse de sensibilité ;
- estimation économique catégorisée ;
- recommandation de test ;
- rapport exportable ;
- authentification et isolation tenant.

## P1 après Product Proof

- suivi d’expérimentations live ;
- suggestion de groupes de contrôle appariés ;
- Decision Ledger ;
- historique des recommandations ;
- commentaires et validation ;
- notifications ;
- comparaison attendu / observé.

## P2 uniquement après preuve

- writeback OneStock ;
- optimisation automatique ;
- temps réel ;
- multi-OMS ;
- agent conversationnel ;
- auto-remédiation ;
- politiques de sourcing, capacité et split shipment.

---

# 12. Backtest historique

Le backtest applique des politiques candidates aux situations observées.

Il compare :

- disponibilité exposée ;
- positions masquées ;
- fulfillment réussi ;
- failed fulfillment ;
- réallocations ;
- annulations ;
- valeur de commande ;
- coût estimé lorsque disponible.

Le backtest ne prétend pas reproduire entièrement le moteur OneStock.

Chaque résultat affiche :

- période ;
- population ;
- taille d’échantillon ;
- métriques observées ;
- hypothèses ;
- variables inconnues ;
- intervalle ou fourchette ;
- biais potentiels.

## Validation temporelle

Le modèle est évalué sur une période distincte de la période d’apprentissage. Les splits aléatoires sont interdits lorsque la saisonnalité ou les changements de politique peuvent contaminer l’évaluation.

---

# 13. Conception des expérimentations live

Le traitement initial porte uniquement sur la politique de dernière unité.

## Méthodes possibles

Selon les données et le réseau :

- magasins appariés ;
- A/B au niveau magasin ;
- switchback temporel ;
- rollout progressif ;
- différence-de-différences ;
- avant / après contrôlé.

OmniOps ne doit pas imposer une seule méthode causale.

## Groupe de contrôle

Le contrôle est sélectionné sur :

- volume de commandes ;
- mix catégorie ;
- pays ;
- profondeur de stock ;
- taux d’échec ;
- saisonnalité ;
- profil de fulfillment.

L’algorithme suggère. L’utilisateur valide.

## Interférence réseau

Le test doit vérifier si le traitement modifie la charge ou les allocations des magasins contrôle. Si OneStock réalloue les commandes entre groupes, le rapport marque l’expérimentation comme potentiellement contaminée.

## Puissance et durée

Avant le lancement, afficher :

- métrique primaire ;
- effet minimal détectable ;
- baseline ;
- taille nécessaire ;
- durée estimée ;
- saisonnalité connue ;
- risque de test sous-puissant.

Un test sous-puissant peut être déclaré inconclusif, jamais positif par défaut.

## Exemple

```text
Treatment : 12 magasins comparables
Control   : 12 magasins comparables
Durée     : 14 à 28 jours
Métrique primaire : fulfillment failure rate
Guardrail : availability loss < 0.5pp
Secondaires : cancellation, reallocation, conversion si disponible
```

## Guardrails

- arrêt si disponibilité baisse au-delà du seuil ;
- investigation si annulation augmente ;
- pas d’extension si échantillon insuffisant ;
- rollback manuel documenté ;
- conservation des résultats négatifs et inconclusifs.

---

# 14. Décision économique

La fonction objectif ne minimise pas seulement les échecs.

```text
Valeur additionnelle de disponibilité
+ échecs évités × contribution attendue
+ économies de réallocation et de transport
- pertes de conversion estimées
- coût d’implémentation
- coût de test
```

Les politiques doivent être présentées sur une frontière :

- disponibilité ;
- échec ;
- positions masquées ;
- valeur économique ;
- incertitude.

Aucun “winner” ne doit être affiché lorsque les différences sont statistiquement ou économiquement indéterminées.

---

# 15. Expérience utilisateur MVP

## Overview

- politique actuelle ;
- performance observée ;
- opportunité identifiée ;
- qualité des données ;
- action suivante.

## Policy Comparison

- politique actuelle ;
- baselines ;
- politique contextuelle ;
- mêmes données et période ;
- trade-offs visibles.

## Backtest

- filtres période, pays, magasin, catégorie ;
- hypothèses ;
- incertitude ;
- variables inconnues ;
- export du rapport.

## Experiment

- hypothèse ;
- traitement ;
- contrôle ;
- métrique primaire ;
- guardrails ;
- puissance ;
- résultat attendu puis observé.

Le chat n’est pas la navigation principale du MVP.

---

# 16. Stratégie IA

Les conclusions quantitatives ne doivent jamais être générées directement par un LLM.

```text
LLM → outil structuré → calcul statistique → evidence → explication
```

## Statistique / ML

- scoring ;
- calibration ;
- segmentation ;
- backtest ;
- estimation d’incertitude ;
- analyse traitement / contrôle.

## LLM

- résumé ;
- explication des drivers ;
- génération d’hypothèses ;
- aide à la formulation d’une politique ;
- interrogation des résultats.

Le LLM ne doit pas choisir une politique sans métriques, baseline et evidence associées.

---

# 17. Go-to-market

## Offre d’entrée

> “Nous analysons 90 jours de données OneStock, reconstruisons votre politique actuelle, la comparons à des règles alternatives et identifions un test contrôlé sur un petit périmètre.”

Le premier rendez-vous doit qualifier :

- une politique modifiable ;
- un problème économique ;
- un accès aux données ;
- un responsable ;
- une possibilité de groupe contrôle.

## Vente

La vente initiale est un audit payé, pas une promesse de plateforme.

Le passage au SaaS intervient si :

- le client lance au moins un test ;
- le backtest est utilisé dans une décision ;
- le résultat réel peut être mesuré ;
- le client veut répéter l’analyse sur d’autres périodes ou politiques.

## Pricing indicatif

- Decision Audit : `€5k–€15k` ;
- Design Partner avec expérimentation : `€10k–€25k` ;
- SaaS Experiment : `€30k–€50k ARR` après preuve ;
- SaaS Decision : `€50k–€80k ARR` avec historique et mesure d’impact.

La tarification dépend de l’ordre annuel, des magasins, marchés, connecteurs et politiques gérées, pas principalement du nombre d’utilisateurs.

---

# 18. Concurrence et marché

Le marché comprend :

- OMS et plateformes d’orchestration ;
- équipes data internes ;
- BI, SQL et data warehouses ;
- intégrateurs et cabinets spécialisés.

L’avantage ne sera pas un algorithme isolé. Il devra venir d’un produit qui combine :

1. données opérationnelles normalisées ;
2. politiques versionnées ;
3. backtests reproductibles ;
4. expérimentation contrôlée ;
5. résultat économique ;
6. mémoire des décisions.

OneStock est le beachhead, pas la destination finale. La priorité reste toutefois de réussir un use case sur OneStock avant d’investir dans Fluent, Manhattan ou un modèle multi-OMS.

---

# 19. Sécurité et intégration

MVP :

- TLS ;
- chiffrement au repos ;
- isolation tenant ;
- gestion des secrets ;
- moindre privilège ;
- journal d’audit ;
- rétention configurable ;
- sauvegardes ;
- suppression de la PII non nécessaire.

Modes d’ingestion acceptés :

- export batch ;
- API ;
- webhook ;
- SFTP ;
- data warehouse.

Le temps réel n’est pas nécessaire pour la première preuve de valeur.

Toute écriture future vers OneStock doit exiger :

- permission explicite ;
- validation ;
- audit log ;
- métadonnées de rollback ;
- politique testée au préalable.

---

# 20. Critères de succès

Après 2 ou 3 design partners :

### Data

- au moins deux clients fournissent les données minimales ;
- les politiques actuelles peuvent être représentées ;
- les métriques critiques sont reproductibles.

### Prediction

- le modèle dynamique bat les baselines sur une période hors entraînement ;
- le lift est stable et économiquement significatif.

### Decision

- au moins une politique alternative est préférée en backtest ;
- au moins un client accepte de la tester.

### Reality

- au moins un test live par design partner ;
- au moins un test montre une amélioration réelle sans violation de guardrail ;
- l’écart entre backtest et résultat réel est documenté.

### Commercial

- deux clients continuent après le pilote ;
- valeur annuelle crédible au moins trois fois supérieure au prix envisagé ;
- au moins une décision opérationnelle modifiée grâce au produit.

---

# 21. Hard Kill Criteria

Arrêter ou changer de wedge si :

- les heuristiques simples égalent le modèle dynamique ;
- les données d’inventory au moment de l’allocation sont systématiquement absentes ;
- aucune politique candidate ne bat la politique actuelle ;
- aucun client n’accepte un test contrôlé ;
- le réseau ne permet pas de construire un contrôle crédible ;
- le test ne peut pas mesurer sa métrique primaire ;
- les gains sont inférieurs au coût du changement ;
- la demande perdue est impossible à encadrer économiquement ;
- OneStock couvre le besoin à un niveau satisfaisant ;
- les analyses sont consultées mais ne modifient jamais les décisions.

---

# 22. Roadmap par preuve

```text
PHASE 0 — Decision Audit
Payer pour mesurer le problème.
        ↓
PHASE 1 — Experiment MVP
Comparer des politiques et recommander un test.
        ↓
PHASE 2 — Live Experiment
Mesurer traitement, contrôle et guardrails.
        ↓
PHASE 3 — Decision Ledger
Conserver politiques, hypothèses et résultats.
        ↓
PHASE 4 — Economics
Enrichir avec coûts, marge et demande.
        ↓
PHASE 5 — Additional Policies
Sourcing, capacité, split shipment et promesse.
        ↓
PHASE 6 — Multi-OMS
Universal Policy Model.
        ↓
PHASE 7 — Governed Action
Writeback après validation répétée.
```

Chaque phase doit être débloquée par une preuve. Une vision plus large ne justifie pas de construire la phase suivante avant cette preuve.

---

# 23. Principe directeur

OmniOps ne doit pas gagner parce que son modèle est plus impressionnant.

Il doit gagner parce que :

> **la décision prise avec OmniOps produit un meilleur résultat que la décision qui aurait été prise sans OmniOps.**

La règle de développement est :

```text
Baseline simple → Politique meilleure → Test contrôlé → Résultat mesuré
```

La question finale de chaque fonctionnalité est :

> **Le retailer a-t-il pris une décision différente grâce à OmniOps, et cette décision a-t-elle produit un résultat mesurablement meilleur ?**

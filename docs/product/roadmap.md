# Roadmap — OmniOps AI

> Feuille de route jalonnée. Cohérente avec le PRD v4 (`prd.md`) et `strategy.md`.
> Statut : **hypothèse de cadence** — les jalons de *preuve* sont fermes, les *dates*
> sont indicatives et ré-ancrées à chaque sortie de phase.

---

## 1. Principe de séquencement

Deux axiomes gouvernent cette roadmap (issus du PRD et de la stratégie) :

1. **Chaque phase est débloquée par une preuve, pas par un calendrier.** On ne
   construit la phase N+1 que si la phase N a produit sa preuve (PRD §22).
2. **Le goulot est le temps fondateur + l'accès client, pas le dev**
   (strategy §2.2). Les dates les plus incertaines sont celles qui dépendent d'un
   tiers (design partner, décideur OneStock), pas celles qui dépendent de code.

L'unité de progression est donc la **preuve**, pas la semaine. Une phase dont le
gate n'est pas franchi ne s'étire pas dans le temps : elle se ré-qualifie ou s'arrête
(cf. PRD §21 — Hard Kill Criteria).

---

## 2. Les 5 preuves (jalons de validation fermes)

> Repris de `strategy.md` §7. Ce sont les seuls jalons qui comptent.

| # | Preuve | Critère de sortie | Débloque |
|---|---|---|---|
| **P1** | Preuve de données | 2 clients fournissent les exports minimaux L1/L2 | Backtest crédible |
| **P2** | Preuve de décision | 1 politique alternative préférée en backtest | Phase 1 — Experiment MVP |
| **P3** | Preuve de réalité | 1 test live, amélioration mesurée, guardrails respectés | Phase 2 — Live Experiment |
| **P4** | Preuve commerciale | 2 clients continuent après pilote, ARR crédible ≥ 3× le prix | SaaS Experiment (€30–50k ARR) |
| **P5** | Preuve de répétabilité | 1 audit mené en < 2 semaines d'effort | Passage à l'échelle |

---

## 3. Calendrier indicatif

> **Ancre : T0 = démarrage de l'exécution Phase 0** (stack tranchée + 1er contact
> design partner). Indicatif : T0 ≈ 1er octobre 2026.

```text
PHASE 0 — Decision Audit          T0      → T0+4m   (oct 2026 → janv 2027)
PHASE 1 — Experiment MVP          T0+4m   → T0+8m   (fév 2027 → mai 2027)
PHASE 2 — Live Experiment         T0+8m   → T0+11m  (juin 2027 → août 2027)
PHASE 3 — Decision Ledger         T0+11m  → T0+14m  (sept 2027 → déc 2027)
PHASE 4 — Economics               T0+14m  → T0+17m  (janv 2028 → mars 2028)
PHASE 5 — Additional Policies     T0+17m  → T0+20m  (avr 2028 → juin 2028)
PHASE 6 — Multi-OMS               T0+20m  → T0+23m  (juil 2028 → sept 2028)
PHASE 7 — Governed Action         T0+23m  → T0+26m  (oct 2028 → déc 2028)
```

Les phases 4→7 sont **indicatives et volontairement lointaines** : elles ne
démarrent que si les preuves P3/P4/P5 sont acquises, et leur périmètre dépendra des
résultats réels. Le détail opérationnel ci-dessous ne porte que sur les phases 0→3.

---

## 4. Phase 0 — Productized Decision Audit

**Objectif : produire P1 (données) et P2 (décision).** Aucun investissement SaaS avant
la sortie de cette phase.

### Sous-jalons

| Jalon | Horizon | Contenu | Dépend de |
|---|---|---|---|
| 0.1 Cadrage design partner | T0 → T0+1m | Clarifier le cadre contractuel Longchamp (non-concurrence, propriété des données, autorisation écrite — strategy §5.4), séparer les rôles, qualifier 2 prospects sur les 5 portes (PRD §5/§17) | Belhaj + décideur client |
| 0.2 Exports reçus | T0+1m → T0+2m | Envoyer `spec-extraction-onestock.md`, recevoir 60–180 j de données L1/L2 (2 clients) | Design partners |
| 0.3 Pipeline v1 | T0+1m → T0+3m | Ingestion batch, modèle normalisé, data quality report, baselines A–D, backtest v0 | Belhaj (dev) |
| 0.4 1er audit | T0+2m → T0+3m | Rapport Phase 0 complet (9 livrables, PRD §9) | 0.2 + 0.3 |
| 0.5 2e audit + backtest comparatif | T0+3m → T0+4m | 2e client audité, politique alternative préférée en backtest | 0.4 |

### Gate de sortie — GO vers Phase 1 (tout ou rien)

- [ ] données techniquement accessibles (≥ 2 clients) ;
- [ ] baseline fiable ;
- [ ] **une politique alternative bat les heuristiques simples** (P2) ;
- [ ] au moins 1 client prêt à tester (responsable + périmètre + groupe contrôle) ;
- [ ] paiement de l'audit ou engagement contractuel ;
- [ ] métrique économique crédible (fourchette + sensibilité, jamais une valeur certaine).

**NO-GO** → ne pas construire le SaaS ; re-qualifier l'offre ou pivoter (PRD §21).
Le pipeline v1 reste un actif réutilisable même en cas de NO-GO (c'est l'enjeu
économique n°1, strategy §2.2).

---

## 5. Phase 1 — Experiment MVP

**Objectif : comparer des politiques et recommander un test.** Uniquement après P2.

- **Livrables P0** (PRD §11) : import batch (API/export/SFTP/warehouse), modèle
  normalisé, data quality report, politiques versionnées, baselines, modèle de
  probabilité de succès, backtest, comparaison de politiques, sensibilité, estimation
  économique catégorisée, recommandation de test, rapport exportable, auth + isolation
  tenant.
- **Gate de sortie** : un client **lance au moins un test live** en s'appuyant sur le
  produit (pas seulement sur un rapport manuel).

---

## 6. Phase 2 — Live Experiment

**Objectif : mesurer traitement / contrôle / guardrails, produire P3.**

- Suivi d'expérimentation live, groupes de contrôle appariés, puissance & durée,
  détection d'interférence réseau, comparaison attendu/observé (PRD §13, §11-P1).
- **Gate de sortie (P3)** : 1 test live → amélioration réelle mesurée, **sans**
  violation de guardrail, avec l'écart backtest/réel documenté.

---

## 7. Phase 3 — Decision Ledger

**Objectif : conserver politiques, hypothèses et résultats (P1 du PRD §11).**

- Historique versionné des politiques, recommandations, commentaires/validation,
  notifications.
- **Gate de sortie** : la mémoire des décisions est réutilisée d'une période à l'autre
  par au moins un client (pas un simple stockage).

---

## 8. Risques de calendrier & mitigations

| Risque | Impact sur le calendrier | Mitigation |
|---|---|---|
| Blocage contractuel Longchamp | 0.1/0.2 glissent de 1–2 mois | Plan B immédiat : client tiers via intégrateur OneStock (strategy §5.3) |
| `inventory_at_allocation` absent | L1 ok, L2 bloqué (audit « dernière unité » impossible) | Reconstruction par snapshots horodatés + `History` (spec §D/§E.5) |
| Aucun client n'accepte un test | Phase 1 sans issue | Ne pas construire le SaaS ; re-qualifier l'offre (PRD §21) |
| Audit > 6 semaines récurrent | Temps fondateur non rentable (P5 menacé) | Productiser le pipeline v1 en priorité absolue (strategy §2.2) |
| Heuristiques simples = modèle dynamique | P2 non atteint | Kill criteria : pivoter le wedge, ne pas forcer le modèle |

---

## 9. Décisions à trancher avant / pendant Phase 0

> Repris de `strategy.md` §8. Chacune débloque un sous-jalon de la Phase 0.

1. **Stack Phase 0** — pipeline analytics Python (à valider avant 0.3).
2. **Géo du 1er design partner** — France/EU (proximité OneStock, réseau) vs UK/US
   (tickets plus élevés).
3. **Prix plancher de l'audit** — €5–8k productisé (plus de preuves, plus rapide) vs
   €10–15k sur-mesure. Recommandation : démarrer à €5–8k.
4. **Canal initial** — intégrateurs OneStock (rapide) vs contenu de preuve anonymisé
   (lent, différenciant). Recommandation : les deux, intégrateurs d'abord.
5. **Longchamp** — feu vert contractuel écrit, ou bascule plan B intégrateur.

---

## 10. Cadence de révision

- Ré-ancrer les dates à **chaque preuve obtenue** (P1→P5), pas à date fixe.
- Une revue de gate à chaque sortie de phase : **GO / NO-GO / RE-QUALIFIER** écrit.
- Toute nouvelle preuve invalide le calendrier en aval et le recalcule depuis la
  dernière phase franchie.

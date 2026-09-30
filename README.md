# OmniOps AI

> **Decision Audit & Policy Experimentation** pour opérations omnicanales (retail OMS).
> Wedge initial : retailers OneStock — politique de disponibilité de la dernière unité.

## Positionnement (1 phrase)

OmniOps vend une **boucle de décision mesurable** — `Données → Baseline → Politiques candidates → Backtest → Test contrôlé → Résultat réel` — pas un score IA ni un dashboard.

## Statut

- [x] PRD v4 — `docs/product/prd.md`
- [x] Strategy & modèle économique — `docs/product/strategy.md`
- [x] Roadmap jalonnée — `docs/product/roadmap.md`
- [ ] Stack technique & architecture (Phase 0 : pipeline analytics Python — à valider)
- [ ] Moteur Phase 0 (Decision Audit) : ingestion, data model, policies, backtest, rapport
- [ ] Product Proof (audit payé qui démontre une opportunité)
- [ ] SaaS Phase 1 (Experiment MVP) — **uniquement après preuve produit**

## Règle d'or du PRD

> Le SaaS ne se construit que si l'audit démontre une opportunité réelle, mesurable et payée.

La règle de développement : `Baseline simple → Politique meilleure → Test contrôlé → Résultat mesuré`.

## Convention du repo

- `docs/product/` — PRD, stratégie, roadmap.
- `src/` — moteur Phase 0 (à créer).
- `tests/` — suite pytest.
- `data/` — exports clients & données (non versionné, cf. `.gitignore`).

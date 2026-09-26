# Fiche d'extraction OneStock — OmniOps AI (audit Phase 0)

> Document de demande de données à destination des design partners / clients.
> Objectif : obtenir les exports OneStock minimaux pour reconstruire la politique de
> disponibilité, établir une baseline et mener un backtest (cf. PRD §9).

## A. Message client (à envoyer tel quel)

> Bonjour [Prénom],
>
> Pour auditer votre politique de disponibilité (masquage / exposition du dernier stock)
> et chiffrer l'écart entre le stock réellement vendable et celui qui échoue à la préparation,
> nous avons besoin d'un extrait **read-only** de vos données OneStock, sur **60 à 180 jours**.
>
> Deux points de réassurance :
> - **Zero network access** : vous nous déposez un export batch — nous ne nous connectons
>   **jamais** à votre OMS, aucun accès à vos serveurs.
> - **Aucune donnée personnelle** : ni nom, ni adresse, ni email de client final.
>   Uniquement références techniques, statuts de fulfillment et niveaux de stock.
>
> Concrètement, il nous faut ~6 fichiers (détail en pièce jointe). Le champ le plus important
> est le **stock disponible au moment de l'allocation** — ou, à défaut, des **snapshots de
> stock horodatés** qui permettent de le reconstruire.
>
> Pouvez-vous nous indiquer qui, côté OMS / omnichannel, est en mesure de produire ces
> exports ? On peut caler un appel de 20 min si utile.

## B. Entités et champs demandés

Les noms OneStock exacts sont à confirmer à l'onboarding ; ci-dessous le **contrat sémantique**
attendu. Un fichier par entité, grain commande/ligne (pas de rapport pré-agrégé).

1. **Magasins / nodes de fulfillment — requis (L1)**
   `node_id` (stable), nom, pays, ville, type (boutique / entrepôt), date d'ouverture, actif (oui/non).

2. **Produits / SKU — requis (L1)**
   `sku_id`, catégorie/famille, prix unitaire, devise.

3. **Commandes — requis (L1)**
   `order_id`, `created_at` (UTC), canal, pays, montant brut, devise, statut.

4. **Lignes de commande + allocation — requis (L1)**
   `order_line_id`, `order_id`, `sku_id`, quantité, `unit_price`,
   `allocated_node_id` (le magasin qui a préparé), `allocation_time` (UTC),
   et **`inventory_at_allocation`** — *optionnel mais critique, c'est le passage L1 → L2*.

5. **Outcome de fulfillment — requis (L1)**
   `order_line_id`, `success` (oui/non), `failure_type` (`no_stock`, `store_miss`, `damaged`, …),
   `completed_at` (UTC).

6. **Snapshots de stock — requis pour L2**
   `node_id`, `sku_id`, `timestamp` (UTC), `quantity`.
   Idéalement journalier + au moment de chaque allocation.

7. **Réallocations — optionnel (L3)**
   `order_line_id`, `from_node_id`, `to_node_id`, `timestamp`, `reason`.

8. **Annulations — optionnel (L3)**
   `order_line_id`, `timestamp`, `reason`.

9. **Politique & buffers actuels — optionnel (L3)**
   Config de disponibilité : buffers, règles de masquage, seuils, **version + date d'effet**.

10. **Métriques de demande / conversion — optionnel (L4)**
    Impressions dispo, recherches, add-to-cart, conversion, sessions, commandes abandonnées,
    demande par canal.

## C. Règles de format & pièges

- CSV ou JSON, **un fichier par entité** ; IDs stables et cohérents entre tous les fichiers (clés de jointure).
- Timestamps **UTC**, ISO 8601.
- **Pas de PII** (ni nom / adresse / email client) — minimalisation RGPD.
- Fenêtre : **60 j minimum, 180 j idéal**.
- ⚠️ Envoyer le **stock brut**, jamais la « disponibilité calculée » par OneStock —
  c'est précisément ce calcul qu'on audite.
- ⚠️ `allocation_time` ≠ date de commande : c'est la date d'**allocation** qui permet de
  reconstruire le stock au moment de la décision.
- ⚠️ Un export agrégé (rapport mensuel) est **inutilisable** — on a besoin du grain commande/ligne.

## D. Le champ critique (à expliquer au client si blocage)

**`inventory_at_allocation`** = le stock réellement disponible au moment où l'OMS a alloué la
commande à un magasin. C'est lui qui sépare un audit « baseline d'échec » (L1) d'un audit
« dernière unité » (L2 — le cœur du wedge). S'il n'est pas exportable tel quel, on le
**reconstruit** à partir des snapshots de stock horodatés — d'où l'importance du fichier #6.

## E. Recette d'extraction OneStock (annexe technique)

> À coller dans l'email au client/design partner — transforme « on vous envoie un fichier »
> en « voici exactement quoi exécuter ». Sources : developers.onestock-retail.com + Confluence
> OneStock (vérifié sept. 2026). Les noms exacts restent à confirmer à l'onboarding (cf. §B).

### E.1 Les 3 mécanismes natifs (et lequel utiliser)

OneStock expose trois modes d'intégration officiels :

1. **APIs synchrones** — lecture temps réel (orders, stock). Utile pour un script d'export.
2. **Webhooks** — événements sortants (`order_state_changed`, `stock_updated`,
   `shipment_created`). Pour le *futur*, pas pour l'historique 60–180 j.
3. **SFTP / batch** — export CSV/JSON en volume. **Recommandé Phase 0** (natif, prouvé,
   compatible zero-network-access).

### E.2 Mapping endpoint → champs de la fiche

| Besoin (fiche §B) | Mécanisme OneStock |
|---|---|
| Commandes + lignes + allocation | `GET /orders`, `/search_orders` (paginé) — `line_item_groups.endpoint_id` (= `allocated_node_id`), `state`, `reason` |
| Outcome de fulfillment | `parcels.state`, `parcels.delivery.origin`, `parcels.date` |
| Stock / inventaire | `GET /stock_export` (sync < 1000 lignes) ou **export async → SFTP CSV** via translator |
| Réallocations / annulations | API `History` (raw log) + webhooks loggés |
| Politique & buffers | config OMC (export manuel, versionné) |

### E.3 Flux SFTP recommandé (détail concret)

1. Le client crée un **translator** (JSON→CSV) : soit les prêts-à-l'emploi
   `default_on_hand_stock_transformer` / `default_future_stock_transformer`, soit un custom
   via `POST /translators` pour obtenir **notre** schéma exact.
2. Le client configure le **SFTP** dans l'OMC (dossier `onestock/` créé automatiquement).
3. Le client déclenche `GET /stock_export` avec `ftp.sftp_name`, `ftp.translator_id`, `ftp.filename`.
4. OneStock génère le CSV **asynchrone** et le pousse sur le SFTP du client.
5. Le client nous dépose les fichiers (notre SFTP / S3 / upload). OmniOps ne touche jamais OneStock.

### E.4 Trois chemins possibles (ordre de préférence)

1. **SFTP push** (recommandé) — ci-dessus.
2. **Script API côté client** — IT client : `POST /login` → token → pull `/orders`,
   `/stock_export`, `/history` → CSV. Le script tourne *chez le client*.
3. **Warehouse client** — si OneStock est déjà mirroré vers Snowflake/BigQuery
   (« data warehouse synchronization »), l'extraction = SQL + export.

### E.5 Le champ critique : `inventory_at_allocation`

Non exposé en direct sur la ligne de commande a priori. Deux pistes (à trancher à l'onboarding) :

1. **API « Time Machine »** (menu doc OneStock : « Time machine Stock v1 ») — si elle rejoue
   l'état du stock passé, c'est la solution idéale. **Vérifier en premier.**
2. **Snapshots de stock horodatés** (`stock_export` quotidien + au moment de l'allocation)
   + log `History` pour interpoler.

### E.6 Le piège à éviter

La **BI Suite (Sisense)** exporte des widgets/dashboards en CSV/Excel — mais c'est de
l'**agrégé**, donc inutilisable (cf. §C). Toujours le grain commande/ligne via API ou SFTP.

### E.7 Checklist à poser au contact OneStock du client

- [ ] `Time Machine` disponible sur ce site ? sémantique exacte (état du stock à une date passée) ?
- [ ] Un champ équivalent à `inventory_at_allocation` est-il exportable via `/orders` ou `History` ?
- [ ] Volume attendu (nb lignes commandes/lignes/stock sur 90–180 j) et rate limits des endpoints ?
- [ ] Translator custom autorisé (`POST /translators`) pour mapper sur notre schéma ?
- [ ] Historique `History` (raw log) : profondeur de rétention (couvre 180 j ?) ?
- [ ] SFTP : OneStock-managed ou self-managed (clé publique à autoriser) ?

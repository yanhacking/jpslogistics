# 🚢 LogiSaaS Platform — Roadmap & Suivi de Projet

<div align="center">

![Statut](https://img.shields.io/badge/Statut-Planification-orange?style=for-the-badge&logo=github)
![Phase](https://img.shields.io/badge/Phase%20Actuelle-1%20%E2%80%94%20Foundation-blue?style=for-the-badge)
![Progression](https://img.shields.io/badge/Progression%20Globale-0%25-red?style=for-the-badge)
![Mise à jour](https://img.shields.io/badge/Mis%20à%20jour-19%20Juin%202026-lightgrey?style=for-the-badge)

**Version 2.0 — Blueprint révisé (Claude_Plan.pdf)**

</div>

---

## 📊 Progression Globale par Phase

| # | Phase | Semaines | Progression | Statut |
|---|-------|----------|-------------|--------|
| 1 | 🏗️ Foundation & Infrastructure | 1 – 2 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 2 | 📦 MVP Core (Mois 0–6) | 3 – 14 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 3 | 🔌 Scale the Modules (Mois 6–14) | 15 – 22 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 4 | 🛡️ Defensibility & Ecosystem (Mois 14–24) | 23 – 28 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 5 | 🚀 Déploiement Production | 29 – 30 | `░░░░░░░░░░` 0% | ⬜ À faire |

---

## 🔍 Légende

| Icône | Signification |
|-------|---------------|
| ✅ `- [x]` | Terminé et testé |
| ⬜ `- [ ]` | À faire |
| ⭐ | Feature UX prioritaire différenciatrice |
| 🆕 | Nouveau vs blueprint v1 |

---

## 🏗️ PHASE 1 — Foundation & Infrastructure `Semaines 1–2`

> **Objectif :** Base technique solide — monorepo, DB, auth, event bus — avant toute feature métier.

### ⚙️ Setup Projet & Monorepo
- [ ] Initialisation monorepo Turborepo (`apps/web`, `apps/api`, `apps/workers`, `packages/`)
- [ ] Docker Compose local (PostgreSQL, Redis, EventBridge ou Kafka)
- [ ] TypeScript strict + ESLint + Prettier partagés via `packages/config`
- [ ] Pipeline CI/CD GitHub Actions (lint → test → build → deploy)
- [ ] Gestion secrets & variables d'environnement
- [ ] Logging structuré (Pino)

### 🗄️ Base de Données — 40+ Tables Prisma
- [ ] Groupe Tenancy — `tenants`, `users`, `roles_permissions`, `sessions`, `audit_logs`
- [ ] Groupe CRM — `contacts`, `contact_persons`, `leads`, `activities`
- [ ] Groupe Pricing — `tariffs`, `rate_components`, `fx_rate_snapshots`, `margin_rules`
- [ ] Groupe Booking — `quote_drafts`, `quotes`, `bookings`
- [ ] Groupe Shipments — `shipments` (mode enum: ocean_fcl | ocean_lcl | air | road | rail)
- [ ] Groupe Shipments — `shipment_events`, `shipment_milestones`, `shipment_containers`, `shipment_cargo_lines`
- [ ] Groupe Documents — `shipping_documents` (HAWB | MAWB | HBL | MBL | BOL | PACKING_LIST | CERT_ORIGIN)
- [ ] Groupe WMS — `warehouses`, `warehouse_locations`, `asns`, `cargo_pieces`, `scan_events`
- [ ] Groupe WMS — `ulds`, `uld_pieces`
- [ ] 🆕 Groupe WMS Cross-dock — `staging_lanes` (`target_dwell_minutes`, `assigned_at`, `status`)
- [ ] 🆕 Groupe Air Charter — `charter_flights`, `charter_allocations`
- [ ] Groupe Billing — `invoices`, `invoice_lines`, `payments`, `carrier_invoices`, `tariff_reconciliation`, `accounting_sync_outbox`
- [ ] Groupe Exceptions — `exception_queue`, `exception_comments`, `escalation_rules`, `rules_engine_configs`
- [ ] Groupe Compliance — `compliance_rules`, `compliance_checks`, `customs_entries`
- [ ] 🆕 Groupe IoT — `telemetry_devices`, `telemetry_readings` (TimescaleDB hypertable)
- [ ] Groupe Intégrations — `edi_raw_messages`, `carrier_connections`, `webhook_subscriptions`, `webhook_deliveries`, `api_keys`, `processed_events`
- [ ] Groupe Notifications — `notification_templates`, `notifications`, `notification_deliveries`
- [ ] Row-Level Security PostgreSQL sur toutes les tables tenant-scoped
- [ ] Middleware Fastify `SET app.current_tenant` par requête
- [ ] Migrations initiales (`prisma migrate dev`) + seed data de démonstration

### 🔐 Authentification & Multi-Tenancy
- [ ] Register / Login email + bcrypt
- [ ] JWT access token (15 min) + refresh token (7 j)
- [ ] SSO OIDC Google Workspace
- [ ] SSO OIDC Microsoft Azure AD
- [ ] 2FA TOTP (Google Authenticator / Authy)
- [ ] Invitation email (token signé 48 h)
- [ ] Reset mot de passe
- [ ] Middleware résolution tenant (sous-domaine ou header `X-Tenant-ID`)
- [ ] Rôles : `ADMIN` | `OPS` | `FINANCE` | `WAREHOUSE` | `VIEWER`
- [ ] IP allowlist configurable par tenant

---

## 📦 PHASE 2 — MVP Core `Mois 0–6 · Semaines 3–14`

> **"Prove the workflow, not the feature list."**
> Ocean + air dès le jour 1. Exception queue = core MVP, pas un stretch goal.

---

### 2.1 👥 CRM & Contacts `Semaine 3`
- [ ] CRUD Contacts (shipper / consignee / agent / carrier / customs broker)
- [ ] Personnes de contact par société
- [ ] Limite crédit & conditions paiement par contact
- [ ] Pipeline leads Kanban (5 colonnes : Prospect → Qualifié → Devis → Négociation → Gagné/Perdu)
- [ ] Historique activités (appel, email, réunion, note interne)
- [ ] Import CSV contacts + fusion doublons
- [ ] Tags & segmentation contacts

---

### 2.2 💰 Tariffs & Pricing `Semaines 3–4`
- [ ] CRUD Tariffs par carrier / lane / mode / période de validité
- [ ] Rate components : `BASE` | `BAF` | `CAF` | `THC` | `PEAK_SEASON` | `SECURITY` | `DTHC` | `ISF` | `AMS`
- [ ] Méthodes de calcul : `PER_CONTAINER` | `PER_KG` | `PERCENT_OF_BASE` | `FLAT`
- [ ] Volume break tiers (tranches tarifaires)
- [ ] Snapshot FX rate au booking (jamais live-floating — quotes historiques reproductibles)
- [ ] Margin rules par client / lane / mode / carrier
- [ ] Calculateur prix itemisé (jamais total seul — ops réconcile par ligne)
- [ ] 🆕 Volumétrique métrique air : `(L × W × H cm) ÷ divisor` (défaut 6000, configurable tenant+carrier)
- [ ] 🆕 Volumétrique impérial air : `(L × W × H inches) ÷ 366` (ou 194 selon carrier)
- [ ] 🆕 `billable_weight = MAX(actual_weight, volumetric_weight)` — convention air standard
- [ ] Unit switcher persistant par user (kg/cm ↔ lb/in) sans rechargement de page
- [ ] Import Excel/CSV tariffs + alertes expiration (T-30j, T-7j)

---

### 2.3 📄 Quoting & Booking `Semaines 4–5`
- [ ] ⭐ Split-pane quote builder (gauche = formulaire, droite = aperçu PDF live)
- [ ] AI quote intake — parsing emails entrants (Gmail API / Graph API → LLM extraction)
- [ ] Score de confiance par champ extrait (champs incertains flagués — jamais auto-envoyés)
- [ ] Review drawer validation avant envoi
- [ ] Calcul quote itemisé avec FX snapshot intégré
- [ ] Génération PDF quote (Puppeteer)
- [ ] Envoi quote par email depuis la plateforme
- [ ] Conversion quote → booking (1 clic)
- [ ] CRUD Bookings — référence ULID (pas auto-increment, évite énumération multi-région)
- [ ] 🆕 Formulaire booking **mode-agnostic** : ocean FCL | ocean LCL | air | road | rail — même form, même state machine
- [ ] 🆕 Mode détermine le jeu de documents : HBL/MBL (ocean) ou HAWB/MAWB (air) — pas le workflow
- [ ] Booking → event `booking_confirmed` → shipment créé → tâches aval déclenchées
- [ ] Export PDF booking confirmation
- [ ] Historique versions quotes

---

### 2.4 🚢 Shipment Operations `Semaines 5–7`

#### State Machine (partagée ocean + air)
```
DRAFT → BOOKED → ORIGIN_PICKUP → EXPORT_CLEARED → IN_TRANSIT
      → ARRIVED_DEST → IMPORT_CLEARED → OUT_FOR_DELIVERY → DELIVERED
                     ↘ EXCEPTION (depuis n'importe quel état)
```

- [ ] ⭐ State machine shipments mode-agnostic (chaque transition = event Kafka/EventBridge + audit log)
- [ ] ⭐ Kanban drag-and-drop par lifecycle (glisser = mise à jour statut + confirmation toast + audit)
- [ ] ⭐ Side-drawer contextuel (tabs : Documents | Événements | Financials | Communications)
- [ ] 🆕 Génération HBL + MBL automatique (ocean)
- [ ] 🆕 Génération HAWB + MAWB basique (air) — **dans MVP scope**
- [ ] e-AWB / IATA Cargo-XML : FWB (Freight Waybill) + FHL (House Manifest)
- [ ] Message adapter layer carrier (Type B EDIFACT-like → `cargo_message` event normalisé)
- [ ] Génération Packing List PDF
- [ ] Génération Certificat d'Origine PDF
- [ ] Gestion parties shipment (shipper, consignee, notify party)
- [ ] Gestion conteneurs & marchandises (numéros conteneur, HS codes, poids, dims)
- [ ] Timeline milestones visuelle par shipment
- [ ] Normalisation timestamps → UTC à la frontière d'ingestion (raw string conservé pour audit)
- [ ] Déduplication milestones multi-sources : clé `(shipment_id, milestone_type, event_time ± tolerance)`
- [ ] Commentaires & mentions @user sur shipments

---

### 2.5 🚨 Exception Queue & Inbox Zero `Semaines 8–9` ⭐ CŒUR MVP

> **"Event → Rule → Task"** — jamais "Event → Dashboard Widget"
> Queue mode-agnostic et source-agnostic : carrier EDI, visibility aggregators, IoT → même enveloppe.

- [ ] Rules engine JSON par tenant (versioned, sans redéploiement)
- [ ] ⭐ Inbox Zero UI : navigation clavier `J/K` (haut/bas) · `E` (résoudre) · `S` (snooze)
- [ ] Groupage par urgence SLA : CRITICAL → HIGH → MEDIUM → LOW
- [ ] Indicateur visuel SLA restant (barre rouge si < 25%)
- [ ] Snooze configurable (1h · 4h · 24h · custom)
- [ ] Réassigner exception à un collègue
- [ ] Auto-escalade Slack/Teams webhook (SLA à 50%)
- [ ] Auto-escalade email (SLA à 75%)
- [ ] Commentaires & historique par exception
- [ ] Types d'exceptions couverts :
  - `DELAY` — Retard vessel / flight
  - `CUSTOMS_HOLD` — Blocage douanier
  - `OVERCHARGE` — Surcharge carrier détectée
  - `DAMAGE` — Dommage cargo
  - `MISSING_DOC` — Document manquant (déclenche upload portail)
  - `COMPLIANCE_BREACH` — Deadline ISF / AMS / ICS2 non respectée
  - `SHORT_SHIP` — Pièces manquantes
  - `CARRIER_INVOICE_VARIANCE` — Écart facture carrier vs contrat
  - 🆕 `TEMP_EXCURSION` — Température reefer hors plage contractuelle (IoT)
  - 🆕 `CHARTER_CAPACITY_BREACH` — Allocation charter dépassant la capacité
  - 🆕 `CROSS_DOCK_DWELL_EXCEEDED` — SLA cross-dock dépassé

---

### 2.6 🏭 WMS — Warehouse Management `Semaines 10–11`
- [ ] Gestion entrepôts, zones, allées, niveaux
- [ ] Plan grille inventory (grid_x / grid_y / grid_z)
- [ ] ASN (Advance Shipment Notices) depuis booking ou EDI 214
- [ ] ⭐ PWA mobile scan réception (BarcodeDetector API — caméra, sans app native)
- [ ] Fallback BLE HID scanner (input caché capturant keystrokes — hardware-agnostic)
- [ ] Mode offline PWA avec sync différée
- [ ] Vibration haptic confirmation scan (mobile)
- [ ] Génération barcodes GS1-128 : `{tenant_prefix}{shipment_id_short}{piece_sequence}{check_digit}`
- [ ] Unicité barcode par tenant + fenêtre glissante 90 jours
- [ ] Algorithme slotting greedy (nearest free bin au outbound dock pour le ship-mode du cargo)
- [ ] Discrepances (short / over / damaged) → tâche auto dans exception queue
- [ ] CQRS : `scan_events` append-only → projector async → `warehouse_locations.occupied_cbm`
- [ ] Optimistic concurrency (version column) — pas de long-held locks
- [ ] Gestion ULDs — cycle de vie indépendant des shipments
- [ ] 🆕 **Cross-dock operations** : inbound scan → match manifest → staging lane assignée
- [ ] 🆕 `dwell_minutes = scanned_out_at − received_at` calculé automatiquement
- [ ] 🆕 Dépassement `target_dwell_minutes` sans scan-out → exception `CROSS_DOCK_DWELL_EXCEEDED`
- [ ] Interface plan d'entrepôt visuel (grille interactive)

---

### 2.7 ✈️ Air Charter Management `Semaines 10–11` 🆕
- [ ] CRUD vols charter (`charter_flights` : flight_no, aircraft_type, capacité kg + CBM, origin, dest, departure_at)
- [ ] Allocations shipments sur charter (`charter_allocations` : allocated_kg, allocated_cbm par shipment)
- [ ] ⭐ Capacity gauge widget (booked vs available kg/CBM) sur page détail charter
- [ ] Hard warning (pas silent block) si nouvelle allocation dépasse capacité
- [ ] Manifest-submission deadline → entrée dans exception/compliance queue
- [ ] Surbook intentionnel possible (overbook vs no-show historique) avec confirmation explicite
- [ ] Statuts charter : `PLANNED` | `MANIFESTED` | `DEPARTED` | `ARRIVED`

---

### 2.8 💳 Billing & Financials `Semaines 12–13`
- [ ] CRUD Invoices multi-devise (`amount_original` + `fx_rate` + `amount_functional`)
- [ ] Invoice lines liées aux `rate_components`
- [ ] Génération PDF invoice (Puppeteer)
- [ ] Envoi invoice par email depuis la plateforme
- [ ] Réception carrier invoices (EDI 210 ou saisie manuelle)
- [ ] ⭐ Réconciliation automatique : `expected_cost` (tariff booking) vs `actual_cost` (carrier invoice) → exception si variance > seuil
- [ ] Stockage haute précision `NUMERIC(14,6)` — banker's rounding uniquement à l'affichage
- [ ] Export GL CSV QuickBooks
- [ ] Export GL CSV Xero
- [ ] Outbox pattern sync comptable (zéro perte d'événements financiers si downstream down)
- [ ] Stripe payment intents portail (server-side)
- [ ] Suivi paiements + réconciliation `invoice_lines.amount_paid`
- [ ] Rapports CA / marge par client / mode / lane / période
- [ ] Factures récurrentes + avoirs (credit notes)

---

### 2.9 🌐 Portail Client Self-Service `Semaines 13–14`
- [ ] Auth portail (token limité — shipper voit uniquement ses shipments où il est listed party)
- [ ] Tracker public par numéro de référence (sans login)
- [ ] Vue "Mes shipments" avec filtres
- [ ] Download documents (URLs S3 signées TTL court — pas de liens publics permanents)
- [ ] 🆕 **Customs clearance status timeline** : filed → under review → released → held
- [ ] 🆕 Si hold doc manquant : shipper upload direct → tâche auto broker/ops
- [ ] 🆕 Droits & taxes en attente affichés avant release (shipper jamais surpris)
- [ ] Paiement factures Stripe (server-side)
- [ ] White-labeling : logo, couleurs, domaine CNAME + wildcard TLS
- [ ] Emails automatiques (milestone, invoice, document disponible)
- [ ] Responsive mobile complet

---

## 🔌 PHASE 3 — Scale the Modules `Mois 6–14 · Semaines 15–22`

---

### 3.1 🌡️ IoT Telemetry Ingestion `Semaines 15–16` 🆕

> Pipeline séparé du bus business-events — IoT = haute fréquence (secondes), 99% ne touche pas OLTP PostgreSQL.

```
Device MQTT → AWS IoT Core / Azure IoT Hub
    → Kinesis Data Streams / Azure Event Hub
    → Stream processor (Kinesis Analytics / Flink)
         ↓ raw readings → TimescaleDB (time-series store)
         ↓ threshold breach seulement
    → event telemetry.reefer.temp_excursion → Kafka/EventBridge
    → exception_queue type TEMP_EXCURSION (même Inbox Zero UI)
```

- [ ] Setup AWS IoT Core ou Azure IoT Hub (MQTT ingestion)
- [ ] Kinesis Data Streams / Azure Event Hub
- [ ] Stream processor Flink / Kinesis Analytics
- [ ] TimescaleDB hypertable pour `telemetry_readings`
- [ ] Table `telemetry_devices` (device_type, serial, shipment_id, tenant_id)
- [ ] Threshold rules configurables par tenant + shipment (plage temp reefer, zone GPS)
- [ ] Breach → event `telemetry.reefer.temp_excursion` → exception_queue
- [ ] Graphique température/humidité sur page shipment (time-series chart)
- [ ] Alertes GPS sortie de zone prédéfinie → exception

### 3.2 📡 EDI Legacy Ingestion `Semaines 15–16`
- [ ] Listener SFTP (entrant carrier)
- [ ] Listener AS2 (bidirectionnel carrier)
- [ ] Stockage raw EDI en Cloudflare R2 (immuable, audit permanent)
- [ ] Parser X12 210 → `billing.carrier_invoice.received`
- [ ] Parser X12 214 → `tracking.milestone.updated`
- [ ] Parser X12 315 → `tracking.milestone.updated`
- [ ] Parser EDIFACT IFTSTA → `tracking.milestone.updated`
- [ ] Parser EDIFACT IFTMIN → `booking.instruction.received`
- [ ] Validation per-message (1 bad record ne bloque pas le batch)
- [ ] Dead-letter topic payloads malformés + auto-ticket ops
- [ ] Replay tooling après correction mapping

### 3.3 🚢 Carriers & Visibility Aggregators `Semaines 16–17`
- [ ] Adaptateur Maersk Tracking API
- [ ] Adaptateur MSC API
- [ ] Adaptateur CMA-CGM API
- [ ] Adaptateur Hapag-Lloyd API
- [ ] Adaptateur DHL Express API
- [ ] Adaptateur FedEx API
- [ ] Adaptateur UPS API
- [ ] Adaptateur Project44 (aggregator — interface partagée `fetch_milestones → canonical_milestone[]`)
- [ ] Adaptateur Vizion (aggregator)
- [ ] Adaptateur CargoAi (aggregator)
- [ ] Déduplication milestones multi-sources : `(shipment_id, milestone_type, event_time ± tolerance)`
- [ ] Worker polling carriers toutes les 2h

### 3.4 💼 Billing Avancée & Intégrations Comptables `Semaine 17`
- [ ] Multi-currency billing complet (flux complexes)
- [ ] QuickBooks Online API (GL sync bidirectionnel)
- [ ] Xero API (GL sync bidirectionnel)
- [ ] Square integration portail
- [ ] Webhooks paiements confirmés → `invoice_lines.amount_paid`

### 3.5 🔗 Open API & Webhooks Publics `Semaines 19–20`
- [ ] REST API v1 publique (`/v1/shipments`, `/v1/bookings`, `/v1/quotes`, `/v1/documents`, `/v1/tracking`)
- [ ] GraphQL pour portail/dashboard (évite over/under-fetching)
- [ ] Subscriptions webhooks par tenant (event_type, URL, secret)
- [ ] Delivery at-least-once + backoff exponentiel (fenêtre max ~24h)
- [ ] Signature HMAC-SHA256 chaque payload
- [ ] `event_id` unique — déduplication côté récepteur via `processed_events`
- [ ] Developer portal + API explorer interactif
- [ ] Scoped API keys (read-only vs write)
- [ ] Versioning `/v1` → `/v2` (sunset policy 12 mois minimum)

### 3.6 📊 Analytics Avancés `Semaine 22`
- [ ] Carrier scorecard (ponctualité %, incidents, coût moyen)
- [ ] Lane analytics (volume, délai moyen, coût moyen, évolution)
- [ ] Revenue analytics (CA, marge par client / mode / lane / période)
- [ ] WMS analytics (dwell time, taux occupation, productivité scan/h)
- [ ] CO2 calculator par shipment
- [ ] Carte shipments in-transit (Mapbox GL)
- [ ] Timeline Gantt par shipment
- [ ] Export rapports Excel/CSV

---

## 🛡️ PHASE 4 — Defensibility & Ecosystem `Mois 14–24 · Semaines 23–28`

### 4.1 🌍 Compliance Engine Généralisé
- [ ] US ISF 10+2 (24h avant chargement port étranger)
- [ ] US AMS manifest (24h avant lading)
- [ ] 🆕 EU ICS2 (Import Control System 2 — pré-arrivée sécurité)
- [ ] Local/destination customs — `jurisdiction` + `deadline_offset` comme data (pas hardcodé)
- [ ] Warnings T-48h → T-24h → Breach (même Inbox Zero ops)
- [ ] Compliance ne requiert pas de déploiement pour ajouter un pays (data row)
- [ ] Validateur HS codes

### 4.2 🤖 ML & IA Avancés
- [ ] ML rate benchmarking (comparer tariff vs marché)
- [ ] Demand-based pricing suggestions
- [ ] Anomaly detection patterns carrier (retards récurrents, surcharges systématiques)
- [ ] AI résumé shipment en langage naturel (état actuel en 2 phrases)
- [ ] ML-based WMS slotting (historique dwell-time)

### 4.3 🌐 Multi-Région & Souveraineté
- [ ] Data residency EU (RGPD — cluster Aurora EU)
- [ ] Data residency APAC
- [ ] `tenants.data_region` comme attribut first-class
- [ ] IAM policy cross-region replication disabled (pas juste convention)
- [ ] Silo escape hatch : grands tenants → schema ou cluster dédié

### 4.4 🔗 Ecosystem Marketplace
- [ ] Open API partner marketplace (app directory)
- [ ] Developer portal public
- [ ] Integrations Project44, Vizion, CargoAi (si pas Phase 3)

---

## 🚀 PHASE 5 — Déploiement Production `Semaines 29–30`
- [ ] Audit sécurité & penetration test
- [ ] Load testing (k6) — 1000 tenants simultanés
- [ ] Disaster recovery plan & runbook
- [ ] Monitoring Datadog / Grafana (SLI/SLO par tenant)
- [ ] Rate limiting par tenant (Upstash Redis)
- [ ] Chiffrement credentials carriers (vault)
- [ ] Backup automatique PostgreSQL (point-in-time recovery)
- [ ] Documentation utilisateur complète
- [ ] Onboarding premier client production

---

## ✨ Fonctionnalités UX Transversales

| Feature | Priorité | Statut |
|---------|----------|--------|
| ⭐ Cmd+K omni-search global (shipments, bookings, contacts, invoices) | HAUTE | ⬜ |
| ⭐ Unit switcher kg/cm ↔ lb/in persistant (user preference, sans reload) | HAUTE | ⬜ |
| ⭐ Volumétrique live (métrique + impérial simultanément) | HAUTE | ⬜ |
| Notifications in-app temps réel (WebSocket/SSE) | HAUTE | ⬜ |
| Mentions @user dans commentaires | MOYENNE | ⬜ |
| Audit log exhaustif (diff old/new values, IP, user) | HAUTE | ⬜ |
| Dark mode | MOYENNE | ⬜ |
| Raccourcis clavier globaux | MOYENNE | ⬜ |
| Vue mobile responsive complète | HAUTE | ⬜ |
| PWA installable (WMS scan principalement) | HAUTE | ⬜ |
| Data export RGPD complet | HAUTE | ⬜ |
| IP allowlist par tenant | MOYENNE | ⬜ |
| 2FA TOTP (Google Authenticator) | HAUTE | ⬜ |
| Chiffrement champs sensibles (credentials carriers) | HAUTE | ⬜ |

---

## 🗄️ État des Tables DB

| Table | Groupe | Créée | Migrée | Testée | RLS |
|-------|--------|:-----:|:------:|:------:|:---:|
| `tenants` | Tenancy | ⬜ | ⬜ | ⬜ | — |
| `users` | Tenancy | ⬜ | ⬜ | ⬜ | ✅ |
| `roles_permissions` | Tenancy | ⬜ | ⬜ | ⬜ | ✅ |
| `audit_logs` | Tenancy | ⬜ | ⬜ | ⬜ | ✅ |
| `contacts` | CRM | ⬜ | ⬜ | ⬜ | ✅ |
| `leads` | CRM | ⬜ | ⬜ | ⬜ | ✅ |
| `activities` | CRM | ⬜ | ⬜ | ⬜ | ✅ |
| `tariffs` | Pricing | ⬜ | ⬜ | ⬜ | ✅ |
| `rate_components` | Pricing | ⬜ | ⬜ | ⬜ | ✅ |
| `fx_rate_snapshots` | Pricing | ⬜ | ⬜ | ⬜ | — |
| `margin_rules` | Pricing | ⬜ | ⬜ | ⬜ | ✅ |
| `quote_drafts` | Booking | ⬜ | ⬜ | ⬜ | ✅ |
| `quotes` | Booking | ⬜ | ⬜ | ⬜ | ✅ |
| `bookings` | Booking | ⬜ | ⬜ | ⬜ | ✅ |
| `shipments` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipment_events` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipment_containers` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipment_cargo_lines` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipping_documents` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `exception_queue` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `exception_comments` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `warehouse_locations` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `cargo_pieces` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `scan_events` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `ulds` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `staging_lanes` 🆕 | WMS Cross-dock | ⬜ | ⬜ | ⬜ | ✅ |
| `charter_flights` 🆕 | Air Charter | ⬜ | ⬜ | ⬜ | ✅ |
| `charter_allocations` 🆕 | Air Charter | ⬜ | ⬜ | ⬜ | ✅ |
| `invoices` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `invoice_lines` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `payments` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `carrier_invoices` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `accounting_sync_outbox` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `compliance_rules` | Compliance | ⬜ | ⬜ | ⬜ | — |
| `compliance_checks` | Compliance | ⬜ | ⬜ | ⬜ | ✅ |
| `customs_entries` | Compliance | ⬜ | ⬜ | ⬜ | ✅ |
| `telemetry_devices` 🆕 | IoT | ⬜ | ⬜ | ⬜ | ✅ |
| `telemetry_readings` 🆕 | IoT TimescaleDB | ⬜ | ⬜ | ⬜ | ✅ |
| `edi_raw_messages` | Intégrations | ⬜ | ⬜ | ⬜ | ✅ |
| `webhook_subscriptions` | Intégrations | ⬜ | ⬜ | ⬜ | ✅ |
| `processed_events` | Intégrations | ⬜ | ⬜ | ⬜ | — |
| `notifications` | Notifs | ⬜ | ⬜ | ⬜ | ✅ |

---

## 🛠️ Stack Technique

| Couche | Technologie | Rôle | Statut |
|--------|------------|------|--------|
| Frontend | Next.js 14 + TypeScript | App Router, SSR | ⬜ |
| UI | shadcn/ui + Radix + Tailwind | Composants accessibles | ⬜ |
| State | Zustand + TanStack Query v5 | State global + server | ⬜ |
| Tables | TanStack Table v8 | Tableaux virtualisés | ⬜ |
| Forms | React Hook Form + Zod | Validation typée | ⬜ |
| Drag & Drop | @dnd-kit | Kanban shipments | ⬜ |
| Animations | Framer Motion | Transitions UX | ⬜ |
| Charts | Recharts | Analytics + time-series | ⬜ |
| Backend | Fastify + Node.js | API REST + WebSocket | ⬜ |
| ORM | Prisma v5 | PostgreSQL + migrations | ⬜ |
| DB | PostgreSQL 15 (Supabase) | Multi-tenant + RLS | ⬜ |
| Time-series | TimescaleDB (ext. PG) | IoT telemetry 🆕 | ⬜ |
| Event Bus | AWS EventBridge (MVP) / Kafka (scale) | Events replayables | ⬜ |
| IoT | AWS IoT Core / Azure IoT Hub | MQTT ingestion 🆕 | ⬜ |
| Stream | Kinesis Analytics / Apache Flink | IoT threshold processing 🆕 | ⬜ |
| Cache | Redis (Upstash) | Sessions, rate limit | ⬜ |
| Jobs BG | BullMQ | PDF, email, EDI, tracking | ⬜ |
| Stockage | Cloudflare R2 | Documents, EDI raw | ⬜ |
| Emails | Resend | Transactionnels | ⬜ |
| Paiements | Stripe | Portail client | ⬜ |
| Recherche | OpenSearch | Omni-search Cmd+K | ⬜ |
| PDF | Puppeteer | HTML → PDF | ⬜ |
| Barcodes | bwip-js | Génération GS1-128 | ⬜ |
| CI/CD | GitHub Actions | Lint → Test → Build → Deploy | ⬜ |
| Frontend Deploy | Vercel | Edge network | ⬜ |
| Backend Deploy | Railway | API + Workers | ⬜ |
| Monorepo | Turborepo | Build cache partagé | ⬜ |

---

## ⚠️ Risques Techniques & Mitigations

| Risque | Impact | Mitigation |
|--------|--------|------------|
| EDI malformé carrier | 🔴 HAUTE | Validation per-message · dead-letter topic · replay tooling |
| Write contention WMS scanning | 🔴 HAUTE | CQRS append-only scan_events · projector async · optimistic concurrency |
| 🆕 IoT telemetry overwhelm OLTP | 🔴 HAUTE | Pipeline séparé Kinesis+Flink · TimescaleDB · seul breach → PostgreSQL |
| Multi-currency rounding drift | 🟡 MOYENNE | NUMERIC(14,6) partout · banker's rounding à l'affichage uniquement |
| Webhook/event replay doubles | 🔴 HAUTE | processed_events table · consumers safely re-runnable |
| Timezone ambiguity milestones | 🟡 MOYENNE | UTC à la frontière d'ingestion · raw string conservé |
| Tenant data leakage cache | 🔴 HAUTE | tenant_id dans toutes cache keys · cache jamais au-dessus résolution tenant |
| Charter oversell | 🟡 MOYENNE | Hard warning UI · manifest deadline dans exception queue |

---

## 🌐 Environnements

| Environnement | URL | Statut |
|--------------|-----|--------|
| Local dev | `http://localhost:3000` | ⬜ |
| Staging | `https://staging.logisaas.app` | ⬜ |
| Production | `https://app.logisaas.app` | ⬜ |
| Portail client | `https://[tenant].logisaas.app` | ⬜ |
| API REST | `https://api.logisaas.app/v1` | ⬜ |
| Suivi client | `https://yanrooven.fr/jpslogistics/` | ✅ En ligne |

---

## 📌 Prochaine Étape

> 🎯 **À démarrer maintenant :** Phase 1 — Monorepo Turborepo + Docker Compose + 40+ tables Prisma + RLS + Auth JWT/SSO

---

<div align="center">

*Document mis à jour automatiquement à chaque push — 🆕 = nouveautés vs blueprint v1*

</div>

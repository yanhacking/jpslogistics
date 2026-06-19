# 🚀 LogiSaaS Platform — Roadmap & Suivi de Projet

<div align="center">

![Statut](https://img.shields.io/badge/Statut-Planification-orange?style=for-the-badge&logo=github)
![Phase](https://img.shields.io/badge/Phase%20Actuelle-1%20%E2%80%94%20Foundation-blue?style=for-the-badge)
![Progression](https://img.shields.io/badge/Progression%20Globale-0%25-red?style=for-the-badge)
![Mise à jour](https://img.shields.io/badge/Mis%20à%20jour-19%20Juin%202026-lightgrey?style=for-the-badge)

</div>

---

## 📊 Progression Globale par Phase

| # | Phase | Semaines | Progression | Statut |
|---|-------|----------|-------------|--------|
| 1 | 🏗️ Foundation & Infrastructure | 1 – 2 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 2 | 📦 Modules Core | 3 – 12 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 3 | 🔌 Intégrations | 13 – 14 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 4 | 🤖 IA & Analytics Avancés | 15 – 18 | `░░░░░░░░░░` 0% | ⬜ À faire |
| 5 | 🚀 Déploiement & Production | 19 – 20 | `░░░░░░░░░░` 0% | ⬜ À faire |

---

## 🔍 Légende

| Icône | Signification |
|-------|---------------|
| ✅ `- [x]` | Terminé et testé |
| 🚧 | En cours de développement |
| ⬜ `- [ ]` | À faire |
| 🔴 | Bloqué — nécessite une action |
| ⏸️ | En pause |
| ⭐ | Feature UX prioritaire (impact fort client) |

---

## 🏗️ PHASE 1 — Foundation & Infrastructure `Semaines 1–2`

> **Objectif :** Poser les bases solides avant tout développement de feature.

### ⚙️ Setup Projet & Monorepo
- [ ] Initialisation monorepo Turborepo (`apps/web`, `apps/api`, `apps/workers`, `packages/`)
- [ ] Configuration Docker Compose (PostgreSQL, Redis, Kafka local)
- [ ] Setup TypeScript strict + ESLint + Prettier (partagés via `packages/config`)
- [ ] Pipeline CI/CD GitHub Actions (lint → test → build → deploy)
- [ ] Gestion variables d'environnement (`.env`, secrets GitHub)
- [ ] Setup logging structuré (Pino)

### 🗄️ Base de Données & Prisma
- [ ] Schema Prisma — Groupe Tenancy (`tenants`, `users`, `roles_permissions`, `sessions`, `audit_logs`)
- [ ] Schema Prisma — Groupe CRM (`contacts`, `contact_persons`, `leads`, `activities`)
- [ ] Schema Prisma — Groupe Pricing (`tariffs`, `rate_components`, `fx_rate_snapshots`, `margin_rules`)
- [ ] Schema Prisma — Groupe Booking (`quote_drafts`, `quotes`, `bookings`)
- [ ] Schema Prisma — Groupe Shipments (`shipments`, `shipment_events`, `shipment_milestones`, `shipment_containers`, `shipment_cargo_lines`, `shipping_documents`)
- [ ] Schema Prisma — Groupe WMS (`warehouses`, `warehouse_locations`, `asns`, `cargo_pieces`, `scan_events`, `ulds`, `uld_pieces`)
- [ ] Schema Prisma — Groupe Billing (`invoices`, `invoice_lines`, `payments`, `carrier_invoices`, `tariff_reconciliation`, `accounting_sync_outbox`)
- [ ] Schema Prisma — Groupe Exceptions (`exception_queue`, `exception_comments`, `escalation_rules`, `rules_engine_configs`)
- [ ] Schema Prisma — Groupe Compliance (`compliance_rules`, `compliance_checks`, `customs_entries`)
- [ ] Schema Prisma — Groupe Intégrations (`edi_raw_messages`, `carrier_connections`, `webhook_subscriptions`, `webhook_deliveries`, `api_keys`, `processed_events`)
- [ ] Schema Prisma — Groupe Notifications (`notification_templates`, `notifications`, `notification_deliveries`)
- [ ] Row-Level Security PostgreSQL (policy `tenant_isolation` sur toutes les tables)
- [ ] Middleware Fastify `SET app.current_tenant` par requête
- [ ] Migrations initiales (`prisma migrate dev`)
- [ ] Seed data (tenant démo + données de test)

### 🔐 Authentification & Multi-Tenancy
- [ ] Register / Login email + mot de passe (bcrypt)
- [ ] JWT access token (15min) + refresh token (7j)
- [ ] SSO OIDC Google Workspace
- [ ] SSO OIDC Microsoft Azure AD
- [ ] 2FA TOTP (Google Authenticator / Authy)
- [ ] Système d'invitation par email (token signé)
- [ ] Reset mot de passe (email + token)
- [ ] Middleware tenant context (résolution par header `X-Tenant-ID` ou sous-domaine)
- [ ] Gestion rôles & permissions (`ADMIN`, `OPS`, `FINANCE`, `WAREHOUSE`, `VIEWER`)
- [ ] IP allowlist par tenant (optionnel)

---

## 📦 PHASE 2 — Modules Core `Semaines 3–12`

---

### 2.1 👥 CRM & Contacts `Semaine 3`

- [ ] CRUD Contacts (types : `shipper`, `consignee`, `agent`, `carrier`, `customs_broker`)
- [ ] Gestion personnes de contact par société
- [ ] Pipeline leads Kanban (colonnes : Prospect → Qualifié → Devis → Négociation → Gagné/Perdu)
- [ ] Historique d'activités (appel, email, réunion, note)
- [ ] Limite crédit & conditions de paiement par contact
- [ ] Import contacts via CSV
- [ ] Fusion doublons contacts
- [ ] Tags & segmentation contacts

---

### 2.2 💰 Tariffs & Pricing `Semaines 3–4`

- [ ] CRUD Tariffs (carrier, lane, mode, période de validité)
- [ ] Rate components multi-types (`BASE`, `BAF`, `CAF`, `THC`, `PEAK_SEASON`, `SECURITY`, `DTHC`, `ISF`, `AMS`)
- [ ] Méthodes de calcul (`PER_CONTAINER`, `PER_KG`, `PERCENT_OF_BASE`, `FLAT`)
- [ ] Volume break tiers (tranches tarifaires)
- [ ] Snapshot FX rate au moment du booking (jamais taux flottant live)
- [ ] Margin rules (par client / lane / mode / carrier)
- [ ] Calculateur prix total itemisé (jamais un seul chiffre, toujours détaillé)
- [ ] Interface gestion tariffs (import Excel/CSV)
- [ ] Alertes expiration tariffs (T-30j, T-7j)
- [ ] Historique versions tariffs

---

### 2.3 📄 Quoting & Booking `Semaines 4–5`

- [ ] ⭐ Split-pane quote builder (gauche = formulaire, droite = aperçu PDF live)
- [ ] AI quote intake — parsing emails entrants (Graph API / Gmail API) via Claude API
- [ ] Score de confiance par champ extrait (les champs incertains sont flagués, jamais auto-envoyés)
- [ ] Review drawer pour valider les quotes AI avant envoi
- [ ] Calcul quote avec breakdown itemisé (currency conversion intégrée)
- [ ] Génération PDF quote (Puppeteer / HTML→PDF)
- [ ] Envoi quote par email depuis la plateforme
- [ ] Conversion quote → booking (1 clic)
- [ ] CRUD Bookings (référence ULID, jamais auto-increment)
- [ ] Formulaire booking multi-modal (ocean FCL/LCL, air, route, rail)
- [ ] Export PDF booking confirmation
- [ ] Historique versions quotes

---

### 2.4 🚢 Shipment Operations `Semaines 5–6`

#### State Machine (8 états + EXCEPTION)
```
DRAFT → BOOKED → ORIGIN_PICKUP → EXPORT_CLEARED → IN_TRANSIT
      → ARRIVED_DEST → IMPORT_CLEARED → OUT_FOR_DELIVERY → DELIVERED
                     ↘ EXCEPTION (depuis n'importe quel état)
```

- [ ] State machine shipments (chaque transition = événement Kafka + audit)
- [ ] ⭐ Kanban drag-and-drop par lifecycle (glisser une carte = mise à jour statut + toast + audit)
- [ ] ⭐ Contextual side-drawer (tabs : Documents | Événements | Financials | Communications)
- [ ] Génération HBL (House Bill of Lading) automatique
- [ ] Génération MBL (Master Bill of Lading) automatique
- [ ] Génération HAWB (House Airway Bill) automatique
- [ ] Génération MAWB (Master Airway Bill) automatique
- [ ] Génération Packing List PDF
- [ ] Génération Certificat d'Origine PDF
- [ ] e-AWB / IATA Cargo-XML (FWB + FHL generation)
- [ ] Message adapter layer carrier (Type B EDIFACT-like + XML normalisé)
- [ ] Gestion parties shipment (shipper, consignee, notify party)
- [ ] Gestion conteneurs & marchandises (HS codes, poids, dims)
- [ ] Timeline milestones visuelle par shipment
- [ ] Déduplication milestones multi-sources (carrier direct + aggregator)
- [ ] Commentaires & mentions @user sur shipments
- [ ] Carte Mapbox in-transit (Phase 2 avancé)

---

### 2.5 🚨 Exception Queue & Inbox Zero `Semaines 7–8` ⭐ PRIORITÉ ABSOLUE

> **C'est LA feature différenciatrice vs CargoWise/Magaya**

- [ ] Rules engine JSON par tenant (versioned, sans redéploiement)
- [ ] Pipeline : Événement → Règle → Tâche (pas Événement → Widget dashboard)
- [ ] Table `exception_queue` complète avec SLA, assignation, statuts
- [ ] ⭐ Inbox Zero UI : navigation clavier `J/K` (haut/bas), `E` (résoudre), `S` (snooze)
- [ ] Groupage par urgence SLA (CRITICAL → HIGH → MEDIUM → LOW)
- [ ] Indicateur visuel SLA restant (barre rouge si < 25% temps)
- [ ] Snooze + choisir durée (1h, 4h, 24h, custom)
- [ ] Réassigner exception à un collègue
- [ ] Auto-escalade : Slack webhook si SLA atteint 50%
- [ ] Auto-escalade : email si SLA atteint 75%
- [ ] Commentaires & historique sur chaque exception
- [ ] Types d'exceptions couverts :
  - `DELAY` — Retard vessel/flight
  - `CUSTOMS_HOLD` — Blocage douanes
  - `OVERCHARGE` — Surcharge carrier détectée
  - `DAMAGE` — Dommage cargo
  - `MISSING_DOC` — Document manquant
  - `COMPLIANCE_BREACH` — Deadline compliance
  - `SHORT_SHIP` — Manque de pièces
  - `CARRIER_INVOICE_VARIANCE` — Écart facture carrier

---

### 2.6 🏭 WMS — Warehouse Management `Semaines 9–10`

- [ ] Gestion entrepôts, zones, allées, niveaux
- [ ] Plan grille inventory (grid_x / grid_y / grid_z)
- [ ] ASN (Advance Shipment Notices) depuis booking ou EDI 214
- [ ] ⭐ PWA mobile scan réception (BarcodeDetector API — caméra, sans app native)
- [ ] Fallback BLE HID scanner (input caché capturant keystrokes)
- [ ] Mode offline PWA avec sync différée
- [ ] Vibration haptic confirmation scan (mobile)
- [ ] Génération barcodes GS1-128 (format `{tenant_prefix}{shipment_id_short}{piece_sequence}{check_digit}`)
- [ ] Unicité barcode par tenant + fenêtre 90 jours
- [ ] Algorithme slotting greedy (suggère bin libre le plus proche du dock)
- [ ] Gestion discrepancies (short / over / damaged) → tâche auto dans exception queue
- [ ] CQRS : `scan_events` (append-only, no contention) → projector async → `warehouse_locations.occupied_cbm`
- [ ] Optimistic concurrency (version column) pour éviter row-lock
- [ ] Gestion ULDs (Unit Load Devices) cycle de vie indépendant
- [ ] Interface plan d'entrepôt visuel (grille interactive)
- [ ] Rapports dwell-time & occupation warehouse

---

### 2.7 💳 Billing & Financials `Semaines 11–12`

- [ ] CRUD Invoices multi-devise (toujours `amount_original` + `fx_rate` + `amount_functional`)
- [ ] Invoice lines avec lien vers `rate_components`
- [ ] Génération PDF invoice (Puppeteer)
- [ ] Envoi invoice par email
- [ ] Réception carrier invoices (EDI 210 ou saisie manuelle)
- [ ] ⭐ Réconciliation automatique : `expected_cost` (tariff booking) vs `actual_cost` (carrier invoice) → exception si variance > seuil
- [ ] Arrondi banker's rounding uniquement à l'affichage (stockage haute précision NUMERIC(14,6))
- [ ] Export GL CSV format QuickBooks
- [ ] Export GL CSV format Xero
- [ ] Outbox pattern sync comptable (jamais de perte d'événements financiers)
- [ ] Paiements Stripe (via portail client — server-side intent, jamais credentials côté portal)
- [ ] Suivi paiements reçus + réconciliation `invoice_lines.amount_paid`
- [ ] Rapports financiers : CA, marge, par client / mode / lane / période
- [ ] Factures récurrentes (frais fixes)
- [ ] Gestion avoirs (credit notes)

---

### 2.8 🌐 Portail Client Self-Service `Semaines 11–12`

- [ ] Auth portail (token limité shipper — voit uniquement ses shipments où il est parti)
- [ ] Tracker public par numéro de référence (sans login)
- [ ] Vue "Mes shipments" avec filtres
- [ ] Download documents (URLs S3 signées TTL court — jamais liens publics permanents)
- [ ] Paiement factures via Stripe (portail appelle `billing-financials`, jamais credentials côté UI)
- [ ] White-labeling : logo, couleurs, domaine CNAME + wildcard TLS
- [ ] Notifications email automatiques (milestone, invoice, document disponible)
- [ ] Historique documents téléchargés
- [ ] Responsive mobile complet

---

## 🔌 PHASE 3 — Intégrations `Semaines 13–14`

---

### 3.1 📡 EDI Legacy Ingestion

- [ ] Listener SFTP (port entrant carrier)
- [ ] Listener AS2 (échange bidirectionnel carrier)
- [ ] Stockage raw EDI en Cloudflare R2 (immuable, audit permanent)
- [ ] Parser X12 210 → `billing.carrier_invoice.received`
- [ ] Parser X12 214 → `tracking.milestone.updated`
- [ ] Parser X12 315 → `tracking.milestone.updated`
- [ ] Parser EDIFACT IFTSTA → `tracking.milestone.updated`
- [ ] Parser EDIFACT IFTMIN → `booking.instruction.received`
- [ ] Validation par message (pas par batch — un mauvais record ne bloque pas les autres)
- [ ] Dead-letter topic payloads malformés (raw préservé)
- [ ] Auto-ticket erreur parsing → exception queue ops
- [ ] Replay tooling une fois le mapping corrigé

### 3.2 🚢 Carriers & Visibility Aggregators

- [ ] Adaptateur Maersk Tracking API
- [ ] Adaptateur MSC API
- [ ] Adaptateur CMA-CGM API
- [ ] Adaptateur Hapag-Lloyd API
- [ ] Adaptateur DHL Express API
- [ ] Adaptateur FedEx API
- [ ] Adaptateur UPS API
- [ ] Adaptateur Project44 (aggregator : `fetch_milestones → canonical_milestone[]`)
- [ ] Adaptateur Vizion (aggregator)
- [ ] Adaptateur CargoAi (aggregator)
- [ ] Normalisation timestamps → UTC à la frontière d'ingestion (jamais en aval)
- [ ] Déduplication milestones multi-sources : clé `(shipment_id, milestone_type, event_time±tolerance)`
- [ ] Worker polling carriers (toutes les 2h)

### 3.3 💼 Comptabilité & Paiements

- [ ] QuickBooks Online API (GL export + sync)
- [ ] Xero API (GL export + sync)
- [ ] Stripe Payment Intents (portail client)
- [ ] Square (optionnel Phase 3)
- [ ] Webhooks paiements confirmés → `invoice_lines.amount_paid`
- [ ] Retry/backoff outbox (downstream API outage = zéro perte)

### 3.4 🔗 Open API & Webhooks Publics

- [ ] REST API v1 publique (`/v1/shipments`, `/v1/bookings`, `/v1/quotes`, `/v1/documents`, `/v1/tracking`)
- [ ] GraphQL pour portail/dashboard (évite over/under-fetching sur vues complexes)
- [ ] Gestion subscriptions webhooks par tenant (`event_type`, URL, secret)
- [ ] Delivery at-least-once + backoff exponentiel (fenêtre max ~24h)
- [ ] Signature HMAC-SHA256 chaque payload
- [ ] `event_id` unique pour déduplication côté récepteur
- [ ] Idempotency keys (`processed_events` — vérification avant toute side effect)
- [ ] Developer portal avec API explorer interactif
- [ ] Scoped API keys (read-only vs write)
- [ ] Versioning `/v1`, `/v2` avec politique sunset 12 mois minimum

---

## 🤖 PHASE 4 — IA & Fonctionnalités Avancées `Semaines 15–18`

### 4.1 Intelligence Artificielle

- [ ] AI quote intake complet (email parsing → champs structurés + scores de confiance)
- [ ] Jamais auto-envoyé — review drawer obligatoire
- [ ] Suggestions routing optimal basées sur historique
- [ ] Anomaly detection (retards récurrents carrier, surcharges systématiques)
- [ ] Résumé shipment en langage naturel (état actuel en 2 phrases)
- [ ] ML rate benchmarking (comparer tariff vs marché)
- [ ] Demand-based pricing suggestions

### 4.2 📊 Analytics Avancés

- [ ] Carrier scorecard (ponctualité %, incidents, coût moyen)
- [ ] Lane analytics (volume, délai moyen, coût moyen, évolution)
- [ ] Revenue analytics (CA, marge, par client / mode / lane / période)
- [ ] WMS analytics (dwell time, taux d'occupation, productivité scanning/heure)
- [ ] CO2 calculator par shipment (footprint carbone)
- [ ] Carte des shipments in-transit (Mapbox GL)
- [ ] Timeline Gantt visuelle par shipment
- [ ] Comparateur routes multi-modal
- [ ] Export rapports Excel/CSV

### 4.3 🛡️ Compliance & Douanes Avancés

- [ ] US ISF 10+2 (Importer Security Filing — 24h avant chargement)
- [ ] US AMS manifest 24h avant lading
- [ ] EU ENS (Entry Summary Declaration)
- [ ] CA ACI (Advance Commercial Information)
- [ ] Warnings multi-seuils : T-48h → T-24h → Breach (même Inbox Zero que les exceptions ops)
- [ ] Compliance risk JAMAIS dans un onglet séparé ignoré
- [ ] Gestion customs entries (import/export/transit)
- [ ] Validateur numéros HS codes

### 4.4 🌍 Multi-Région & Souveraineté Données

- [ ] Data residency EU (RGPD — cluster Aurora EU)
- [ ] Data residency APAC
- [ ] `tenants.data_region` comme attribut first-class
- [ ] IAM policy cross-region replication disabled (pas juste convention)
- [ ] Silo escape hatch : grands tenants → schema ou cluster dédié

---

## ✨ Fonctionnalités UX Transversales

| Feature | Priorité | Statut |
|---------|----------|--------|
| ⭐ Cmd+K omni-search (shipments, bookings, contacts, invoices) | HAUTE | ⬜ |
| ⭐ Unit switcher kg/cm ↔ lb/in persistant (user preference) | HAUTE | ⬜ |
| ⭐ Volumétrique live (L×W×H / 6000 air, /5000 ocean) | HAUTE | ⬜ |
| Notifications in-app temps réel (WebSocket/SSE) | HAUTE | ⬜ |
| Mentions @user dans commentaires | MOYENNE | ⬜ |
| Audit log exhaustif (diff old/new values, IP) | HAUTE | ⬜ |
| Dark mode | MOYENNE | ⬜ |
| Raccourcis clavier globaux | MOYENNE | ⬜ |
| Vue mobile responsive complète | HAUTE | ⬜ |
| PWA installable (WMS principalement) | HAUTE | ⬜ |
| Data export RGPD complet | HAUTE | ⬜ |
| IP allowlist par tenant | MOYENNE | ⬜ |
| 2FA TOTP (Google Authenticator) | HAUTE | ⬜ |
| Chiffrement champs sensibles (credentials carriers) | HAUTE | ⬜ |

---

## 🗄️ Base de Données — État des Tables (22 tables + groupes)

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
| `quotes` | Booking | ⬜ | ⬜ | ⬜ | ✅ |
| `bookings` | Booking | ⬜ | ⬜ | ⬜ | ✅ |
| `shipments` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipment_events` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipment_containers` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `shipping_documents` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `exception_queue` | Core | ⬜ | ⬜ | ⬜ | ✅ |
| `warehouse_locations` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `cargo_pieces` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `scan_events` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `ulds` | WMS | ⬜ | ⬜ | ⬜ | ✅ |
| `invoices` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `invoice_lines` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `carrier_invoices` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `accounting_sync_outbox` | Billing | ⬜ | ⬜ | ⬜ | ✅ |
| `compliance_rules` | Compliance | ⬜ | ⬜ | ⬜ | — |
| `compliance_checks` | Compliance | ⬜ | ⬜ | ⬜ | ✅ |
| `edi_raw_messages` | Intégrations | ⬜ | ⬜ | ⬜ | ✅ |
| `webhook_subscriptions` | Intégrations | ⬜ | ⬜ | ⬜ | ✅ |
| `processed_events` | Intégrations | ⬜ | ⬜ | ⬜ | — |
| `notifications` | Notifs | ⬜ | ⬜ | ⬜ | ✅ |

---

## 🛠️ Stack Technique — État d'Installation

| Couche | Technologie | Version | Installé | Configuré |
|--------|------------|---------|:--------:|:---------:|
| **Frontend** | Next.js | 14.x | ⬜ | ⬜ |
| **Language** | TypeScript | 5.x | ⬜ | ⬜ |
| **UI Components** | shadcn/ui + Radix | latest | ⬜ | ⬜ |
| **Styles** | Tailwind CSS | 3.x | ⬜ | ⬜ |
| **State global** | Zustand | 4.x | ⬜ | ⬜ |
| **Server state** | TanStack Query | 5.x | ⬜ | ⬜ |
| **Tables** | TanStack Table | 8.x | ⬜ | ⬜ |
| **Formulaires** | React Hook Form + Zod | latest | ⬜ | ⬜ |
| **Drag & Drop** | @dnd-kit | latest | ⬜ | ⬜ |
| **Animations** | Framer Motion | latest | ⬜ | ⬜ |
| **Charts** | Recharts | 2.x | ⬜ | ⬜ |
| **Backend** | Fastify | 4.x | ⬜ | ⬜ |
| **ORM** | Prisma | 5.x | ⬜ | ⬜ |
| **Base de données** | PostgreSQL (Supabase) | 15 | ⬜ | ⬜ |
| **Cache** | Redis (Upstash) | latest | ⬜ | ⬜ |
| **Event Bus** | Kafka (Upstash) | latest | ⬜ | ⬜ |
| **Jobs BG** | BullMQ | latest | ⬜ | ⬜ |
| **Stockage** | Cloudflare R2 | — | ⬜ | ⬜ |
| **Emails** | Resend | latest | ⬜ | ⬜ |
| **Paiements** | Stripe | latest | ⬜ | ⬜ |
| **Recherche** | OpenSearch | latest | ⬜ | ⬜ |
| **PDF** | Puppeteer | latest | ⬜ | ⬜ |
| **Barcodes** | Sharp + bwip-js | latest | ⬜ | ⬜ |
| **CI/CD** | GitHub Actions | — | ⬜ | ⬜ |
| **Deploy Frontend** | Vercel | — | ⬜ | ⬜ |
| **Deploy Backend** | Railway | — | ⬜ | ⬜ |
| **Monorepo** | Turborepo | latest | ⬜ | ⬜ |

---

## 🚀 Environnements de Déploiement

| Environnement | URL | Statut |
|--------------|-----|--------|
| Local dev | `http://localhost:3000` | ⬜ |
| Staging | `https://staging.logisaas.app` | ⬜ |
| Production | `https://app.logisaas.app` | ⬜ |
| Portail client | `https://[tenant].logisaas.app` | ⬜ |
| API REST | `https://api.logisaas.app/v1` | ⬜ |
| Developer portal | `https://developers.logisaas.app` | ⬜ |

---

## 📌 Prochaine Étape Immédiate

> 🎯 **À démarrer** : Phase 1 — Initialisation monorepo Turborepo + Docker Compose + Schema Prisma

---

## 📞 Contacts Projet

| Rôle | Nom | Contact |
|------|-----|---------|
| Développeur principal | — | — |
| Chef de projet | — | — |
| Client | — | — |

---

<div align="center">

*Document mis à jour par l'équipe de développement à chaque avancement*
*Les checkboxes `- [x]` sont cochées au fur et à mesure du développement*

</div>

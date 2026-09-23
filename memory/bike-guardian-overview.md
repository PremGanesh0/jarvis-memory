# Bike Guardian — Project Overview

> Project reference document. Last updated: 2026-09-23

---

## Project Identity

- **GitHub:** https://github.com/PremGanesh0/bike_guardian
- **Repo Size:** 38.8 MB
- **Created:** 2023-11-01
- **Last Push:** 2026-09-21
- **Primary Language:** Dart (Flutter)
- **Stars:** 1
- **Open Issues:** 2 (#6, #8)
- **Default Branch:** main

---

## What It Is

BikeGuardian is a **comprehensive motorcycle companion app** for India, combining:

1. **Digital document management** — bike ownership papers, license, pollution certs
2. **Emergency QR code system** — vital info for first responders in accidents
3. **Police access** — passcode-based document verification during traffic stops
4. **Ride tracking & trip planning** — GPS rides, multi-day tours, route planning
5. **Garage/vehicle management** — bikes, odometer, service logs, fuel tracking
6. **Social features** — group rides, open rides, chat, follower graph
7. **Crash detection & SOS** — accelerometer-based crash detection with emergency dispatch

---

## Architecture

```
bike_guardian/
├── mobile/                    # Flutter app (MVVM/BLoC)
│   ├── lib/
│   │   ├── core/              # constants, DI, services, utils
│   │   ├── data/              # repositories, API clients
│   │   ├── routes/            # 11 route modules (auth, chat, docs, garage,
│   │   │                       navigation, onboarding, settings, social,
│   │   │                       sos, trip, unknown)
│   │   ├── ui/                # features, theme, widgets
│   │   └── app.dart / main.dart
│   ├── android/, ios/, web/, integration_test/
│   └── pubspec.yaml
├── engine/                    # NestJS monorepo (microservices)
│   ├── apps/
│   │   ├── gateway/           # NestJS/Fastify REST API (/api/v1)
│   │   ├── identity-service/  # Auth, users, IAM
│   │   ├── emergency-service/ # Documents, QR, consent (BR-D)
│   │   ├── garage-service/    # Bikes & garage (BR-G)
│   │   ├── trip-service/      # Trips, rides, media (BR-T)
│   │   ├── social-service/    # Posts & follow chain (BR-P)
│   │   ├── sos-service/       # Crash detection & SOS (BR-C)
│   │   ├── notifications-service/
│   │   ├── monetization-service/
│   │   ├── platform-service/
│   │   └── storage-service/   # Public media read-through
│   ├── packages/
│   │   ├── common/, grpc/, queue/, redis/, shared-auth/, storage/
│   │   └── connectors/        # Shared .proto contracts (TypeScript)
│   └── turbo.json / docker-compose.yml
├── brain/                     # Obsidian Second Brain (558 notes)
│   ├── 00_Inbox/              # Capture notes (200+ scoping docs)
│   ├── 01_Human_Documents/    # Requirements, user stories, specs
│   ├── 02_AI_Documents/       # Architecture, HLD, DB schemas, ops guides
│   ├── 03_Logs/               # Testing logs, completion logs, bug tracker
│   ├── 04_Roadmaps_Sprints/   # Sprint board, roadmap, project tasks
│   ├── 05_Resources/          # Templates, tag taxonomy, operating manual
│   └── 06_Agent_Memory/       # AI agent learned facts
├── admin/                      # Flutter Web admin portal
├── sales/                      # Next.js marketing site (planned/backlog)
├── .agents/                    # AI agent config
├── .mcp.json                   # MCP server config
├── CLAUDE.md / RULES.md        # Project rules for AI agents
└── scripts/                    # brain:index, brain:doctor, etc.
```

---

## Key Tech Decisions

- **Mobile:** Flutter + BLoC/Cubit (MVVM), Firebase Auth, Firestore, Firebase Storage
- **Backend:** NestJS + Fastify + gRPC + MongoDB (one DB per service) + Redis (queues + auth cache)
- **API:** Single gateway HTTP entry point; all internal comms over gRPC via shared `.proto` contracts
- **Auth:** Redis-cached JWT state; cache miss falls back to gRPC call to identity-service (not HTTP)
- **Ride recording:** Offline-first local persistence (SQLite/Hive TBD), GPS sampling, sync on reconnect
- **Media:** MinIO/S3 presigned PUT uploads, public read-through via storage-service
- **Docs:** Obsidian Second Brain with kanban plugin, dataview, MCP connector (Cursor + Hermes)

---

## Current Status (as of 2026-09-15)

### System Status

| Component | Status |
|---|---|
| **Rider Mobile App** (`mobile/`) | 🔵 In Active Build |
| **Backend Microservices** (`engine/apps/`) | 🟢 Architecture Migrated |
| **API Gateway** | 🟢 Functional |
| **Admin Web Portal** (`admin/`) | 🟢 Functional |
| **Marketing Site** (`sales/`) | 🔴 Backlog (Next.js) |
| **Obsidian Second Brain** (`brain/`) | 🟢 Live — Path B semantic index ready |

### Open Work (17 items on Sprint Kanban)

**PM Stage 6 HOLD (not releasing):**
- US-221/222 — Open Ride read-only detail vs host edit screens
- US-217/218 — Group planned Start Ride → solo shell + live peers
- US-213/214 — Open Ride invite host copy + Accept/Reject
- US-196 — 1:1 DM reopen (send/photo/presence failed → US-211/212)
- US-208 — In-ride POI pin (BR-P-05)
- US-209/210 — Ride this route + route fidelity
- US-206 — In-ride camera + media pins
- US-205 — Rest-stop persist
- US-201..204 — Crash/SOS Phase B–D
- US-195..200 — Search/DM/activity/Open Ride invite E2E
- US-191..194 — Open Ride group chat + live map
- US-187..190 — Open Ride polish + Indian numbers

**QA Stage 5 PASS-WITH-FINDINGS:**
- US-219 — Open to Ride listings in Home feed (672/672 Flutter tests, 3 non-blocking findings)

**Planning (Tier 3):**
- US-215/216 — Open Ride linked plan routeId + map polyline
- US-211/212 — Followed-private 1:1 DM + WhatsApp-like send slice

### Recently Shipped
- **Cursor MCP + Obsidian Path B** — MCP Connector 2.7.0, `search_vault_smart` live (483 notes)
- **US-177..180** — Rider Route & Multi-Day Trip Planning Engine (waypoints, checkpoints, traffic delays, live progress HUD)
- **US-171/172** — Ride Lifecycle Engine (start/progress/pause/resume/complete)
- **US-173..176** — Multi-Day Trips & Checkpoint Geofencing
- **US-164** — In-Ride Fuel Logging & Expense Linking
- **Emergency SOS, Document Vault & Garage Fuel Intelligence** — full implementation, 610/610 tests green
- **US-133** — Live MongoDB Atlas credential unstage & `.gitignore` hardening (CRITICAL security fix)
- **US-121** — Server-side magic-byte/MIME validation on all upload routes
- **US-119** — Emergency profiles concurrent-create race fix
- **US-124** — DOCUMENT_EXPIRY cron timezone fix (Asia/Kolkata)

### High-Level Metrics (2026-08-19)
- **Total tracked:** 44 features/tickets
- **LIVE/DEPLOYED:** 19 (43.2%)
- **SIGNED OFF / READY TO MERGE:** 19 (43.2%)
- **PLANNING:** 2
- **BLOCKED (external):** 3
- **REOPENED/BLOCKED (infra):** 2
- **Overall:** ~86%

---

## Open Issues

| # | Title | State | Created |
|---|---|---|---|
| #6 | Need clarity: BR-M1 Ride Vault retention sweep (US-170) + abandoned-ride sweep ticket collision | open | 2026-09-08 |
| #8 | Nightly health check failing | open | 2026-09-08 |

---

## Contact

- **Email:** premganesh2655@gmail.com
- **App Store:** [Bike Guardian on iOS](https://apps.apple.com/se/app/bike-guardian/id6787766705) (published Jul 25, 2026)

---

*This document is a summary. Full detail lives in the repo's `brain/` second brain (558 notes) and the `PROJECT_STATUS.md` / `PROJECT_MASTER_TRACKER.md` dashboards.*

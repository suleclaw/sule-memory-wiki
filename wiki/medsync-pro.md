---
title: MedSync Pro
created: 2026-03-30
updated: 2026-04-08
tags: [project, healthcare, react, supabase, booking]
related: []
---

# MedSync Pro

**Medication sync platform for clinics — helps patients manage repeat prescriptions and appointment bookings.**

## Overview

- **Repo:** https://github.com/oracleot/medsync-pro
- **Local:** ~/projects/medsync-pro
- **Stack:** React + Vite + TypeScript + Supabase (medsync schema)
- **Channel:** #medsync-pro (Discord 1488159963477184615)
- **Linear:** MedSync Pro project (6f7ae5e2) — Sule Orchestrator team

## Current Status (2026-04-08)

| Issue | Title | State |
|-------|-------|-------|
| SUL-5 | Phase 1 MVP: Implement booking feature (3 tables + API) | ✅ Done |
| SUL-7 | Phase 2: Configure Resend for email notifications | 📋 Backlog |

## What We Built

### Phase 1 — Booking Feature (Done ✅)

**3 new tables:**
- `appointments` — core booking records
- `doctor_availability` — weekly recurring availability windows
- `booking_modality` — negotiation model (patient proposes, doctor confirms)

**Booking flow (Option C — Negotiation):**
1. Patient proposes appointment slot(s)
2. Doctor receives notification
3. Doctor confirms or rejects
4. Patient notified of outcome

**Files changed:**
- Database migrations in supabase/migrations/
- New API endpoints for booking CRUD
- Frontend booking UI components

### Phase 2 — Email Notifications (Backlog)
- Resend integration for email
- Notify patients when: booking requested, confirmed, rejected, rescheduled
- Notify doctors when: new booking request, booking cancelled

## Design Docs

- `docs/booking-design.md` — full booking feature design (32KB)
- `docs/SPEC.md` — implementation spec
- `docs/IMPLEMENTATION_TRACKER.md` — progress tracking
- `docs/NEW_FEATURES_SPEC.md` — future feature specs

## Tech Notes

- Supabase schema: `medsync` (not public)
- Auth: email/password (auth.users)
- Booking modality: Option C (negotiation model)
- Row-level security applied to all new tables

## Repository

```bash
git clone https://github.com/oracleot/medsync-pro.git ~/projects/medsync-pro
cd ~/projects/medsync-pro
npm install
npm run dev
```

## Next Steps

1. **Phase 2** — Resend email notifications (SUL-7)
2. **Testing** — E2E tests for booking flow
3. **Edge cases** — doctor rejects, patient cancels mid-negotiation

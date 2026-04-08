---
title: Suleclaw Agency
created: 2026-04-08
updated: 2026-04-08
tags: [project, website, agency, nextjs, deployed]
related: [[q2-goals-2026]]
---

# Suleclaw Agency

**Website:** https://suleclaw-agency.vercel.app
**Repo:** https://github.com/suleclaw/suleclaw-agency
**Status:** Shipped ✅

---

## Overview

Creative agency website showcasing Suleclaw's web development, design, and branding services. Positioned as a premium, high-design agency.

---

## Tech Stack

- **Framework:** Next.js 16
- **Language:** TypeScript
- **Styling:** Tailwind CSS v4
- **Components:** @base-ui/react + Framer Motion
- **Icons:** Lucide
- **Email:** Nodemailer (contact form)
- **Deployment:** Vercel Hobby

---

## Design System — Industrial Noir

Aesthetic direction established in PR #3 (2026-03-31).

| Element | Choice |
|---------|--------|
| **Primary Font** | Instrument Sans |
| **Mono Font** | JetBrains Mono |
| **Display Font** | Syne |
| **Accent Color** | Amber glow system |
| **Texture** | Noise grain overlay |
| **Cards** | Glassmorphism with blur |
| **Layouts** | Asymmetric, bold |

---

## Development History

| Date | Event |
|------|-------|
| 2026-03-31 | V1 shipped — Industrial Noir redesign (PR #3 merged) |
| 2026-03-31 | All review fixes applied (hero font size, project card icon, accordion caret) |
| 2026-03-30 | Pre-design work, frontend-design skill mandatory for UI tasks |

---

## Key Files

```
suleclaw-agency/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── globals.css
├── components/
│   ├── ui/ (base components)
│   └── sections/ (page sections)
├── lib/
│   └── utils.ts
├── public/
│   └── og-image.png
└── docs/
    ├── suleclaw-agency/specs/
    └── suleclaw-agency/design/
```

---

## Workflow Artifacts

- `workflow-artifacts/` — retros, design reviews
- `docs/suleclaw-agency/specs/` — technical specs
- `docs/suleclaw-agency/design/` — design docs

---

## Linear Project

**SUL-33** (archived) — Original Suleclaw Agency issue in Linear

---

## Next Sprint (Backlog)

- [ ] Logo SVG redesign
- [ ] Aceternity UI homepage revamp
- [ ] WhatsApp button fix
- [ ] Footer rework
- [ ] Quick View hover fix
- [ ] Mobile hamburger full-height menu
- [ ] File refactors (200-line rule enforcement)

---

*Last updated: 2026-04-08*

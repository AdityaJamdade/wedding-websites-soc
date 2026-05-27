# Phase Plans — Build Sequence & Cascade Logic

> These are the working development plans. Open the relevant file while actively building that tier.
> Check off items as you complete them. Each file is a living document.

---

## Build Order (Top-Down — always)

```
1. Bespoke   → v5.0-bespoke/       PHASE_01_BESPOKE.md
2. Luxe      → v4.0-luxe/          PHASE_02_LUXE.md
3. Prestige  → v3.0 (existing)     PHASE_03_PRESTIGE.md   ← migrate + polish only
4. Signature → v2.0 (existing)     PHASE_04_SIGNATURE.md  ← migrate only
5. Essential → v2.0 (existing)     PHASE_05_ESSENTIAL.md  ← same migration as Signature
```

Prestige, Signature, and Essential are **production-ready**. Their phase plans are short — mainly the Formspree → EmailJS migration plus demo refresh.

---

## The Cascade Rule

Every feature in a lower tier was built first in Bespoke. When building Luxe, you are not adding new things — you are taking the Bespoke feature set and simplifying or removing specific pieces.

```
BESPOKE  →  multi-page + GSAP + Google Sheets + Bespoke styles + white-label + 6mo hosting
           ↓  remove: multi-page, GSAP, Google Sheets, white-label, 6mo hosting, Bespoke styles
LUXE     →  single-page + Framer Motion + EmailJS-only + Luxe styles + password + particles + guestbook
           ↓  remove: password, particles, guestbook, Luxe styles, domain auto-included
PRESTIGE →  single-page + Framer Motion + video hero + Prestige styles + 11 sections
           ↓  remove: video hero, Prestige styles, premium presets
SIGNATURE → single-page + Framer Motion + countdown + 11 sections + standard styles
           ↓  remove: 6 sections, countdown
ESSENTIAL → single-page + Framer Motion + 5 sections + standard styles
```

---

## Reference Codebases

When building each tier, **read the existing Prestige v3.0 code first**. It is the most complete working template and has solved most of the problems you'll encounter (scroll behaviour, section registry, preset resolution, ThemeProvider, Nav, etc.).

| What you need | Where it lives |
|---|---|
| Section architecture | `wedding-invitation-websites-3.0/template/src/App.jsx` |
| Preset resolution | `wedding-invitation-websites-3.0/template/src/config/presets.js` |
| ThemeProvider | `wedding-invitation-websites-3.0/template/src/providers/ThemeProvider.jsx` |
| Nav / Footer | `wedding-invitation-websites-3.0/template/src/shell/` |
| Any section component | `wedding-invitation-websites-3.0/template/src/sections/` |
| Design style CSS | `wedding-invitation-websites-3.0/template/src/styles/design-styles/` |
| Hero with video | `wedding-invitation-websites-3.0/template/src/sections/Hero.jsx` |
| Countdown | `wedding-invitation-websites-3.0/template/src/ui/` |

---

## Task Tracking Convention

When you start a phase, mark it `IN PROGRESS`. When done, change `[ ]` to `[x]`.

Example:
```
## Phase 1 — Foundation  [IN PROGRESS]
- [x] Next.js project setup
- [x] Tailwind config
- [ ] Deploy to Vercel
```

Also update the task list in Cowork when a full phase is done.

---

## Status Overview

| Tier | Plan File | Status |
|---|---|---|
| Bespoke | `PHASE_01_BESPOKE.md` | 🔲 Not started |
| Luxe | `PHASE_02_LUXE.md` | 🔲 Not started |
| Prestige | `PHASE_03_PRESTIGE.md` | 🔲 Not started |
| Signature | `PHASE_04_SIGNATURE.md` | 🔲 Not started |
| Essential | `PHASE_05_ESSENTIAL.md` | 🔲 Not started |

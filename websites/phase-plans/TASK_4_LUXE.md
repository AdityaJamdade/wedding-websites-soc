# Phase Plan — Luxe (v4.0)

> **Cascade rule:** Luxe is Bespoke with multi-page, GSAP, Google Sheets, and Bespoke-exclusive styles removed. Everything else is either kept as-is or adapted to the single-page Vite architecture.

**Codebase:** `wedding-websites/v4.0-luxe/`
**Framework:** React 19 + Vite (same pattern as Prestige v3.0)
**Base:** Start from `wedding-invitation-websites-3.0/template/` — copy and build on top.
**Reference while building:** Keep `wedding-invitation-websites-3.0/` open. The section components, preset resolution, ThemeProvider, and Nav are all already solved there.

---

## What Luxe Has vs Bespoke

| Feature | Bespoke | Luxe |
|---|---|---|
| Architecture | Multi-page (Next.js) | Single-page scroll (Vite) ← simpler |
| Animations | GSAP ScrollTrigger | Framer Motion only ← simpler |
| Hero types | Video + Parallax3D + Image | Video + Image ← remove Parallax3D |
| Design styles | 3 Bespoke-exclusive | 5 Luxe-exclusive (different set) |
| RSVP data | Google Sheets | EmailJS email only ← simpler |
| Guestbook | Google Sheets | EmailJS email only ← simpler |
| White-label | Always | No — subtle footer credit |
| 6mo hosting | Always | Not included |
| Google Sheets | Yes | No |

**Luxe adds vs Prestige:** password gate, particle system, guestbook section, 5 new design styles, 10 new presets, EmailJS migration.

---

## Phase 1 — Project Setup  `[ ]`

**Estimated time: 0.5 day**

- [ ] Copy `wedding-invitation-websites-3.0/template/` → `v4.0-luxe/template/` as the starting base
  ```bash
  cp -r wedding-invitation-websites-3.0/template/ v4.0-luxe/template/
  ```
- [ ] Update `template/package.json`:
  - Bump version to `4.0.0`
  - Add `"@emailjs/browser": "^4.0.0"`
  - Remove Formspree from any comments
- [ ] Update `template/vite.config.js` — already written, but confirm CLIENT alias resolves correctly
- [ ] Run `npm install` inside `v4.0-luxe/template/`
- [ ] Run `CLIENT=_demo-luxe npm run dev` — confirm app boots (will show Prestige demo content initially)
- [ ] Add `clients/_demo-luxe/appsettings.js` — already written, confirm it loads

---

## Phase 2 — Formspree → EmailJS Migration  `[ ]`

**Estimated time: 0.5 day**
**This is also the migration needed for Prestige and Signature — do Luxe first, then copy the pattern.**

- [ ] Add `lib/emailjs.js` to `v4.0-luxe/template/src/lib/` (already written in scaffolding)
- [ ] Update `sections/RSVP.jsx`:
  - Remove: `formspreeId` prop usage, `fetch('https://formspree.io/...')`
  - Add: `import emailjs from '@emailjs/browser'` and `sendEmail(cfg.rsvp.emailjs, {...})`
  - Keep the exact same form layout — only the submit handler changes
  - Test with a real EmailJS account
- [ ] Update `sections/Music.jsx` — same migration
- [ ] Confirm `_demo-luxe/appsettings.js` has `rsvp.emailjs` + `music.emailjs` keys (already does)
- [ ] Remove `formspreeId` from the Prestige appsettings schema comments (no longer needed)

---

## Phase 3 — Password Gate  `[ ]`

**Estimated time: 0.5 day**

- [ ] Add `lib/password.js` (already written in scaffolding — `isUnlocked`, `verifyAndUnlock`)
- [ ] Add `ui/PasswordGate.jsx` (already written in scaffolding)
- [ ] Update `App.jsx` to wrap the section renderer in `<PasswordGate>` when `config.access?.protected` is true (already in the scaffolded `App.jsx`)
- [ ] Confirm `_demo-luxe/appsettings.js` has `access.protected: true` and a valid `passwordHash`
- [ ] Generate the correct hash for `"blossom2027"`:
  ```bash
  node -e "crypto.subtle.digest('SHA-256', new TextEncoder().encode('blossom2027')).then(b => console.log(Array.from(new Uint8Array(b)).map(x=>x.toString(16).padStart(2,'0')).join('')))"
  ```
- [ ] Update hash in `_demo-luxe/appsettings.js`
- [ ] Test: wrong password shows error, correct password unlocks, refresh keeps unlocked

---

## Phase 4 — Particle System  `[ ]`

**Estimated time: 0.5 day**

- [ ] Add `lib/particles.js` (already written in scaffolding)
- [ ] Add `ui/ParticleCanvas.jsx` (already written in scaffolding)
- [ ] Update `sections/Hero.jsx` (ported from Prestige):
  - Add `<ParticleCanvas>` as an absolute overlay inside the hero container
  - Read `cfg.hero.particles`, `cfg.hero.particleCount`, `cfg.hero.particleColor` from config
  - Canvas must sit above the video/image but below the text content (`z-index: 2` for canvas, `z-index: 3` for text)
  - Reference: Prestige Hero.jsx for the existing overlay + content layout
- [ ] Test petals, stars, fireflies, snow on the demo client
- [ ] Verify particles run at 60fps on a mid-range device (reduce default count if needed)
- [ ] Confirm `hero.particles: null` hides the canvas with no errors

---

## Phase 5 — Guestbook Section  `[ ]`

**Estimated time: 1 day**

- [ ] Create `sections/Guestbook.jsx`:
  - Top half: display `config.guestbook.approvedEntries` — name + message cards, FadeIn stagger
  - Bottom half: submission form — name + message fields → `sendEmail(cfg.guestbook.emailjs, {...})`
  - Success state: "Thank you — your message has been sent!"
  - Layout: match the card style of existing sections (uses same CSS variables)
- [ ] Add `'guestbook': Guestbook` to the SECTIONS map in `App.jsx` (already in scaffolded App.jsx)
- [ ] Confirm `_demo-luxe/appsettings.js` has 3 `guestbook.approvedEntries` pre-filled
- [ ] Test: submit → EmailJS fires → success message shown
- [ ] Style: the guestbook entry cards should feel warm and personal, not like a form response

---

## Phase 6 — 5 Exclusive Luxe Design Styles  `[ ]`

**Estimated time: 2.5 days**
**Reference:** `wedding-invitation-websites-3.0/template/src/styles/design-styles/` — study how aurora-glass.css and velvet-night.css work, then build on top.

- [ ] `styles/design-styles/sakura-drift.css`:
  - Soft blush-pink palette, watercolour-washed section backgrounds
  - Headings in a delicate serif, faint pink shadow
  - Cards: translucent white glass with pink-tinted border
  - Dividers: a row of tiny petal SVG ornaments (inline SVG in CSS `content`)
  - Gallery: rounded corners, soft shadow
- [ ] `styles/design-styles/grand-hotel.css`:
  - Art Deco geometry — thin gold lines, chevron and diamond dividers
  - Headings: tall sans-serif, tracked caps, gold text shadow
  - Cards: deep surface colour, gold border, corner ornaments
  - Section label: a bordered box (Art Deco badge style)
  - Buttons: rectangular with Art Deco border pattern
- [ ] `styles/design-styles/modern-celestial.css`:
  - Dark background, thin constellation line-art dividers (SVG-based)
  - Headings: clean modern sans-serif, slightly spaced
  - Cards: very subtle glass, thin light border
  - Highlight colour: thin gold underline on headings rather than full colour change
  - Tone: quiet, editorial, dark luxury
- [ ] `styles/design-styles/rose-garden-glass.css`:
  - Frosted glass panels — `backdrop-filter: blur(20px)` on all cards
  - Blush/pink background tint, white glass overlay
  - Section backgrounds: can reference a provided garden photo via `--section-bg-image` CSS var
  - Headings: large italic serif, rose-gold colour
  - Very feminine, layered, beautiful
- [ ] `styles/design-styles/editorial-luxe.css`:
  - High contrast — near-black or white background
  - Headings: extremely large (clamp 5rem–12rem), ultra-thin weight (100)
  - No decorative elements — pure typography and layout
  - One-colour accent (gold) used only on the most important element per section
  - Inspired by Bottega Veneta or Maison Margiela website aesthetic
- [ ] Update `config/presets.js` — all 37 presets already defined, confirm the 5 new style classes are wired correctly
- [ ] Test each style by switching `preset` in `_demo-luxe/appsettings.js`

---

## Phase 7 — Demo Client: Isabelle & Mateo  `[ ]`

**Estimated time: 1 day**
**Goal:** A stunning demo that shows why Luxe is worth $1,997.

- [ ] Preset: `sakuraBloom` — cherry petals + soft blush + Japanese garden
- [ ] Fill all 12 sections in `clients/_demo-luxe/appsettings.js` (already partially done):
  - [ ] Story: 4 milestones, well-written prose
  - [ ] Schedule: 5 items (Arrive → Ceremony → Cocktails → Dinner → Last Dance)
  - [ ] Gallery: 6 Unsplash photos (Japanese garden / cherry blossom theme)
  - [ ] Registry: 2 stores
  - [ ] Location: venue description + map embed + transport
  - [ ] Accommodation: 2 hotels with promo codes
  - [ ] FAQ: 5 Q&As
  - [ ] Guestbook: 3 approved entries
- [ ] `hero.particles: 'petals'`, `hero.particleCount: 40`
- [ ] `hero.videoUrl: null` (image hero for the demo — simpler to showcase)
- [ ] Deploy to Vercel at `demo-luxe.vercel.app`
- [ ] Test full guest flow: password (`blossom2027`) → all 12 sections → RSVP submit

---

## Phase 8 — Polish  `[ ]`

**Estimated time: 0.5 day**

- [ ] Mobile audit: all sections work at 375px
- [ ] Particle performance: 60fps target — if dropping frames, reduce default count in `particles.js`
- [ ] Password gate: test on iPhone Safari (sessionStorage behaves differently in private browsing — warn couple)
- [ ] Guestbook: test submit → EmailJS sends
- [ ] Check all 5 new design styles look correct on mobile
- [ ] Run Lighthouse — target Performance ≥ 85
- [ ] Update `TIER_02_LUXE.md` status to ✅ Production
- [ ] Update `PLANS.md` Luxe status to ✅
- [ ] Mark Phase 2 Luxe complete in Cowork task list

---

## Estimated Timeline

| Phase | Work | Days |
|---|---|---|
| 1 | Setup (fork from Prestige) | 0.5 |
| 2 | EmailJS migration | 0.5 |
| 3 | Password gate | 0.5 |
| 4 | Particle system | 0.5 |
| 5 | Guestbook section | 1 |
| 6 | 5 new design styles | 2.5 |
| 7 | Demo client | 1 |
| 8 | Polish | 0.5 |
| **Total** | | **~7 days** |

# Phase Plan — Bespoke (v5.0)

> **The philosophy:** Bespoke is NOT a fully custom build per client. It is the most premium template — designed so well that clients don't need to ask for changes. The differentiation from Luxe comes from multi-page architecture, GSAP-quality animations, three exclusive design styles, and white-glove service. The template does the heavy lifting. The client sends content; we configure and deploy.

**Codebase:** `wedding-websites/v5.0-bespoke/`
**Framework:** Next.js 14 (App Router)
**Reference:** Read `wedding-invitation-websites-3.0/` thoroughly before starting — section components, ThemeProvider, Nav, preset resolution all have working implementations to port from.

---

## What Makes Bespoke Different from Luxe

| Feature | Bespoke | Luxe |
|---|---|---|
| Architecture | Multi-page (separate URL per section) | Single-page scroll |
| Animations | GSAP + ScrollTrigger | Framer Motion only |
| Hero | Video or CSS 3D parallax | Video or image |
| Design styles | 3 exclusive Bespoke-only styles | 5 Luxe styles (not shared) |
| RSVP data | Google Sheets (couple owns a spreadsheet) | Email notification only |
| White-label | Always — zero attribution | Optional |
| Custom domain | Always included | Always included |
| Hosting | 6 months included | 1 year standard |
| Guestbook | Yes (Google Sheets) | Yes (EmailJS only) |
| Password | Yes | Yes |
| Particles | Yes | Yes |

**The client experience:** Couple has a consultation call → we pick the right preset → they send content → we configure and deploy in 5 days. They get a password-protected, white-label multi-page website that looks completely different from any other wedding website they've seen.

---

## Exclusive Bespoke Design Styles (3)

These 3 styles are ONLY in Bespoke. They are more dramatic and cinematic than anything in Luxe or below.

| Key | Class | Vibe | Best preset |
|---|---|---|---|
| `cinematicNoir` | `style-cinematic-noir` | Film noir — deep black, warm gold, dramatic oversized type, feels like a movie poster | `noirGala` |
| `grandRomance` | `style-grand-romance` | Ultra-luxe editorial — deep midnight + rose gold, big photography, generous whitespace | `midnightRose` |
| `minimalistLux` | `style-minimalist-lux` | Swiss editorial luxury — maximum whitespace, single strong accent, feels like a luxury brand | `linenGold` |

### Bespoke Colour Schemes (3 exclusive)

| Key | Primary | Background | Vibe |
|---|---|---|---|
| `noirGold` | `#C9A96E` | `#050507` | Near-black + champagne gold — cinematic |
| `midnightRose` | `#C4748A` | `#08060E` | Deep near-black + rose gold |
| `linenWhite` | `#8B7355` | `#F8F5EE` | Off-white linen + warm tan — editorial luxury |

### Bespoke-Exclusive Presets (6)

| # | Key | Style | Scheme | Vibe |
|---|---|---|---|---|
| 38 | `noirGala` | cinematicNoir | noirGold | Black-tie, cinematic, dramatic |
| 39 | `midnightRose` | grandRomance | midnightRose | Deep romantic, editorial |
| 40 | `linenGold` | minimalistLux | linenWhite | Editorial luxury, clean |
| 41 | `noirElegance` | cinematicNoir | midnight | Dramatic dark, sophisticated |
| 42 | `roseEditorial` | grandRomance | dustyRose | Softer romance, light editorial |
| 43 | `pureMinimal` | minimalistLux | ivory | Clean ivory, maximum whitespace |

---

## Phase 1 — Foundation  `[ ]`

**Estimated time: 1 day**
**Goal:** Next.js app boots, config loader works, deploy pipeline live.

- [ ] Confirm `v5.0-bespoke/template/package.json` has all deps (`next`, `gsap`, `framer-motion`, `@emailjs/browser`)
- [ ] Run `npm install` inside `v5.0-bespoke/template/`
- [ ] Verify `CLIENT=_demo-bespoke npm run dev` starts without errors
- [ ] Confirm `lib/config.js` correctly resolves `clients/_demo-bespoke/appsettings.js`
- [ ] Confirm `app/layout.jsx` renders with `DesignTokenApplier` + `PasswordGateWrapper`
- [ ] Create `tailwind.config.js` with content paths
- [ ] Create `postcss.config.js`
- [ ] Deploy empty app to Vercel → confirm CI pipeline works
- [ ] Verify `NEXT_PUBLIC_CLIENT` env var is set correctly on Vercel

---

## Phase 2 — Shared Utilities  `[ ]`

**Estimated time: 1 day**
**Goal:** All lib files working and tested in isolation.

- [ ] `lib/emailjs.js` — already exists, verify it works with a test send
- [ ] `lib/password.js` — already exists, test `hashPassword` + `verifyAndUnlock`
- [ ] `lib/particles.js` — already exists, confirm it works on a canvas element
- [ ] `lib/sheets.js` — already exists, test with a real Google Apps Script deployment
- [ ] Create `lib/gsap.js`:
  ```js
  // Registers GSAP plugins. Import once in root layout or per-page.
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);
  export { gsap, ScrollTrigger };
  ```
- [ ] `components/ui/ParticleCanvas.jsx` — port from `v4.0-luxe/template/src/ui/ParticleCanvas.jsx`
- [ ] `components/ui/FadeIn.jsx` — GSAP-based version (not Framer Motion):
  - Use `useGSAP` hook with ScrollTrigger for `opacity: 0 → 1, y: 30 → 0`
  - Falls back gracefully if GSAP not loaded
- [ ] `components/ui/Countdown.jsx` — port from `v4.0-luxe/template/src/ui/Countdown.jsx`
- [ ] `components/ui/ScrollReveal.jsx` — GSAP ScrollTrigger wrapper for stagger animations
- [ ] Run `scripts/hashPassword.mjs "riviera2027"` → paste hash into `_demo-bespoke/appsettings.js`

---

## Phase 3 — Layout & Navigation  `[ ]`

**Estimated time: 0.5 day**
**Goal:** Navbar works across all pages, page transitions feel smooth.

- [ ] `components/layout/Navbar.jsx`:
  - Fixed top, transparent → frosted-glass on scroll
  - Couple names as brand mark (centre)
  - Links to each enabled page (from `config.pages`)
  - Mobile: hamburger → full-screen menu overlay
  - Reference: `wedding-invitation-websites-3.0/template/src/shell/Nav.jsx`
- [ ] `components/layout/Footer.jsx`:
  - Couple names + date, minimal
  - No attribution (white-label always on for Bespoke)
- [ ] `components/layout/PageTransition.jsx`:
  - Framer Motion `AnimatePresence` wrapper
  - Subtle fade + slight upward slide between pages
  - Used in `app/layout.jsx` wrapping `{children}`
- [ ] Wire `PageTransition` into `app/layout.jsx`
- [ ] Test navigation between 2 pages

---

## Phase 4 — Hero Components  `[ ]`

**Estimated time: 1 day**
**Goal:** Three hero types fully working, particles overlay correctly.

- [ ] `components/hero/VideoHero.jsx`:
  - Full-screen `<video>` background, muted, autoplay, loop, playsInline
  - Colour overlay div at `hero.overlayOpacity`
  - `<ParticleCanvas>` absolute overlay on top of video
  - Children (couple names, countdown) render above particles at `z-index: 3`
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/Hero.jsx` for the overlay + content layout
  - Add `preload="none"` + poster image for fast initial paint
- [ ] `components/hero/ParallaxHero.jsx`:
  - CSS 3D layered depth: background image + midground overlay + foreground text at different `translateZ` values
  - GSAP ScrollTrigger: background moves at 0.3x scroll speed, midground at 0.6x, text at 1x
  - Fallback to `ImageHero` if `window.matchMedia('prefers-reduced-motion')` is set
- [ ] `components/hero/ImageHero.jsx`:
  - Static background, same overlay + content layout as VideoHero
  - Used when `hero.type === 'image'`
- [ ] Wire hero selection into `app/page.jsx` (already scaffolded — just needs real components)
- [ ] Test all 3 hero types using `_demo-bespoke/appsettings.js`

---

## Phase 5 — Section Pages  `[ ]`

**Estimated time: 3 days**
**Goal:** All 12 pages render correctly with real content from appsettings.js.

**Reference for every section:** Read the matching file in `wedding-invitation-websites-3.0/template/src/sections/` first. Port the structure and styling, then upgrade with GSAP and multi-page layout adjustments.

- [ ] `app/page.jsx` (Home/Hero) — wire VideoHero + ParticleCanvas + Countdown (largely done)
- [ ] `app/story/page.jsx`:
  - Timeline layout: alternating left/right milestones on desktop, vertical stack on mobile
  - GSAP: each milestone animates in as it enters viewport (ScrollTrigger stagger)
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/Story.jsx`
- [ ] `app/details/page.jsx`:
  - Full-screen section with event info grid
  - GSAP fade-in stagger on each detail card
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/Details.jsx`
- [ ] `app/schedule/page.jsx`:
  - Vertical timeline with times + titles
  - GSAP: timeline line draws down as user scrolls
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/Schedule.jsx`
- [ ] `app/gallery/page.jsx`:
  - Masonry grid (CSS columns, no JS library)
  - GSAP stagger reveal as images enter viewport
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/Gallery.jsx`
- [ ] `app/rsvp/page.jsx`:
  - Form → `emailjs.send()` + `sendToSheets()` on submit
  - Confirmation state on success
  - Reference: `wedding-invitation-websites-3.0/template/src/sections/RSVP.jsx` for form layout (replace Formspree with EmailJS + sheets)
- [ ] `app/registry/page.jsx` — Reference: `Registry.jsx`
- [ ] `app/location/page.jsx` — Google Maps iframe embed from config, Reference: `Location.jsx`
- [ ] `app/accommodation/page.jsx` — Hotel cards, Reference: `Accommodation.jsx`
- [ ] `app/music/page.jsx` — Song request form → EmailJS only, Reference: `Music.jsx`
- [ ] `app/faq/page.jsx` — Animated accordion (CSS + Framer Motion height transition), Reference: `FAQ.jsx`
- [ ] `app/guestbook/page.jsx`:
  - Form → EmailJS + `sendToSheets('Guestbook')`
  - Display `config.guestbook.approvedEntries` (static, from config)
  - No real-time display needed — entries appear after couple approves and we redeploy

---

## Phase 6 — 3 Exclusive Bespoke Design Styles  `[ ]`

**Estimated time: 2 days**
**Goal:** Three CSS files that make the site look completely unlike any lower tier.

**Reference:** Read `wedding-invitation-websites-3.0/template/src/styles/design-styles/` for patterns — how each style uses CSS variables, glass effects, shadow systems. Then go further.

- [ ] `src/styles/design-styles/cinematic-noir.css`:
  - Cards: no background, just a fine gold border — `border: 1px solid rgba(201,169,110,0.3)`
  - Section headings: oversized (clamp 4rem–9rem), thin weight (100–200), uppercase, tracked
  - Section dividers: a single horizontal gold line, no floral ornaments
  - Hero overlay: heavier (0.6+), text in stark white or gold
  - Nav: fully transparent with gold underline on active link
  - Buttons: outline-only, gold border, fills on hover
  - Tone: a film poster, not a wedding site
- [ ] `src/styles/design-styles/grand-romance.css`:
  - Cards: subtle deep-surface background with a faint rose-gold glow on hover
  - Headings: large italic serif, generous letter-spacing
  - Section layout: full-bleed background image behind each section with overlay (set via CSS var `--section-bg`)
  - Milestone items: large numbers in faint background, content overlaid
  - Tone: deep romantic editorial — think Vogue, not Pinterest
- [ ] `src/styles/design-styles/minimalist-lux.css`:
  - Light background (`--color-bg: #F8F5EE`)
  - Zero decorative elements — no florals, no dividers, just space
  - Headings: ultra-large, light weight, generous top/bottom margin
  - Content: single-column, max-width 680px, centred
  - Accent: one strong colour used sparingly — on key headings and CTAs only
  - Tone: a luxury fashion brand, not a wedding template
- [ ] `src/styles/globals.css` — add style class activators (same pattern as Prestige)
- [ ] `src/config/presets.js` — add the 6 Bespoke presets (#38–43), 3 colour schemes, 3 style classes
- [ ] `src/components/layout/DesignTokenApplier.jsx` — confirm it handles `linenWhite` (light bg) without text contrast issues
- [ ] Test all 3 styles on the demo client by changing `preset` in appsettings.js

---

## Phase 7 — Google Sheets RSVP & Guestbook  `[ ]`

**Estimated time: 0.5 day**
**Goal:** RSVPs and guestbook entries arrive in a real Google Sheet.

- [ ] Deploy `scripts/sheets-webhook.gs` to a test Google Sheet (use your own account)
- [ ] Set `rsvp.sheetsWebhookUrl` in `_demo-bespoke/appsettings.js` to the test deployment URL
- [ ] Set `guestbook.sheetsWebhookUrl` to the same URL
- [ ] Submit a test RSVP → confirm row appears in the sheet's "RSVPs" tab
- [ ] Submit a test guestbook entry → confirm row appears in "Guestbook" tab
- [ ] Test that `sendToSheets(null, ...)` gracefully no-ops (for clients who skip Sheets)
- [ ] Write the 2-minute client Google Sheets setup instructions (already in `TIER.md` — confirm accuracy)

---

## Phase 8 — Demo Client: Aria & Sebastian  `[ ]`

**Estimated time: 1.5 days**
**Goal:** A jaw-dropping live demo that IS the sales pitch for Bespoke.

**Demo brief:** French Riviera. Villa Ephrussi de Rothschild. Deep navy + champagne gold. Cinematic. Film-poster energy.

- [ ] Choose preset: `noirGala` (cinematicNoir style + noirGold scheme)
- [ ] Fill `clients/_demo-bespoke/appsettings.js` completely:
  - [ ] All story milestones (5 entries, well-written copy)
  - [ ] Schedule (6 items)
  - [ ] Gallery (6 photo URLs — curated Unsplash, French Riviera / villa theme)
  - [ ] Registry (2 entries)
  - [ ] Location (venue description + transport notes)
  - [ ] Accommodation (2 hotels)
  - [ ] FAQ (6 Q&As)
  - [ ] Guestbook (3 pre-approved entries)
- [ ] `hero.type: 'video'` with a cinematic landscape clip
- [ ] `hero.particles: 'stars'` with gold particle colour
- [ ] `access.protected: true` → password `riviera2027`
- [ ] Set `emailjs` keys (use a shared demo EmailJS account — not the client's)
- [ ] Deploy to Vercel at `demo-bespoke.vercel.app` or similar
- [ ] Share URL — test full guest flow: password → browse → RSVP

---

## Phase 9 — Polish & Launch  `[ ]`

**Estimated time: 1 day**

- [ ] Mobile audit: all pages look correct on 375px, 390px, 428px viewports
- [ ] Performance: run Lighthouse — target LCP < 2.5s, CLS < 0.1
- [ ] Video hero: confirm autoplay works on iOS Safari (muted + playsInline required)
- [ ] Particles: confirm 60fps on mid-range Android (reduce count if needed)
- [ ] Password gate: test correct + incorrect password, sessionStorage persistence across tabs
- [ ] RSVP: submit test entry → confirm EmailJS sends + Sheets row appears
- [ ] Guestbook: submit entry → confirm EmailJS sends
- [ ] All pages reachable from nav on mobile
- [ ] Page transitions working on all major browsers
- [ ] Update `TIER_01_BESPOKE.md` status to ✅ Production
- [ ] Update `PLANS.md` Bespoke status to ✅
- [ ] Mark Phase 1 Bespoke complete in Cowork task list

---

## Files to Create (Complete List)

```
v5.0-bespoke/template/
  tailwind.config.js                          Phase 1
  postcss.config.js                           Phase 1
  src/
    lib/
      gsap.js                                 Phase 2
    components/
      ui/
        ParticleCanvas.jsx                    Phase 2
        FadeIn.jsx          (GSAP version)    Phase 2
        Countdown.jsx                         Phase 2
        ScrollReveal.jsx                      Phase 2
      hero/
        VideoHero.jsx                         Phase 4
        ParallaxHero.jsx                      Phase 4
        ImageHero.jsx                         Phase 4
      layout/
        Navbar.jsx                            Phase 3
        Footer.jsx                            Phase 3
        PageTransition.jsx                    Phase 3
    app/
      story/page.jsx                          Phase 5
      details/page.jsx                        Phase 5
      schedule/page.jsx                       Phase 5
      gallery/page.jsx                        Phase 5
      rsvp/page.jsx                           Phase 5
      registry/page.jsx                       Phase 5
      location/page.jsx                       Phase 5
      accommodation/page.jsx                  Phase 5
      music/page.jsx                          Phase 5
      faq/page.jsx                            Phase 5
      guestbook/page.jsx                      Phase 5
    config/
      presets.js                              Phase 6
    styles/
      design-styles/
        cinematic-noir.css                    Phase 6
        grand-romance.css                     Phase 6
        minimalist-lux.css                    Phase 6
```

### Already exists (from scaffolding):
- `lib/config.js`, `lib/emailjs.js`, `lib/password.js`, `lib/particles.js`, `lib/sheets.js`
- `app/layout.jsx`, `app/page.jsx`
- `components/layout/DesignTokenApplier.jsx`, `PasswordGateWrapper.jsx`
- `components/ui/PasswordGate.jsx`
- `styles/globals.css`
- `scripts/sheets-webhook.gs`, `scripts/hashPassword.mjs`
- `clients/_demo-bespoke/appsettings.js`

---

## Estimated Timeline

| Phase | Work | Days |
|---|---|---|
| 1 | Foundation | 1 |
| 2 | Shared utilities | 1 |
| 3 | Layout & nav | 0.5 |
| 4 | Hero components | 1 |
| 5 | Section pages (12) | 3 |
| 6 | 3 exclusive design styles | 2 |
| 7 | Google Sheets | 0.5 |
| 8 | Demo client | 1.5 |
| 9 | Polish & launch | 1 |
| **Total** | | **~11–13 days** |

# Tier 1 — Bespoke

> Fully custom multi-page wedding website built from scratch for each client. No template constraints. The crown jewel — not yet announced publicly.

---

## Status
- **Build status:** Not started
- **Codebase:** `v5.0-bespoke/` *(to be created in `wedding-websites/`)*
- **Framework:** Next.js 14 (App Router)

---

## Pricing
- **Range:** $5,000 – $15,000
- **Average target:** ~$8,000 per client
- **Payment:** 50% upfront via Stripe, 50% on delivery
- **Turnaround:** 5–7 business days from content receipt

---

## What's Included

### Architecture
- Multi-page Next.js 14 app — separate pages per section (not a single-page scroll)
- Custom domain setup included (e.g. `sophia-and-alex.com`)
- Password-protected access — invite-only
- 6 months fully managed hosting on Vercel
- White-label — zero attribution, no "Made by" anywhere
- Mobile-first, sub-2s load time on all pages

### Sections (all custom-designed per client)
- Hero (full-screen, 3D parallax or video)
- Our Story (scroll-driven animated timeline)
- Wedding Details
- Schedule (day-of timeline)
- Gallery (masonry or editorial layout)
- RSVP
- Registry
- Location (with custom illustrated venue map)
- Accommodation
- Music Requests
- FAQ
- Digital Guestbook (guests leave messages + emoji reactions)
- Live stream embed section *(optional — for remote guests)*
- Post-wedding gallery *(unlocked after the wedding)*

### Visual Experience
- Fully custom design — no template, matched to couple's exact brief
- GSAP + ScrollTrigger for scroll-driven animations
- 3D parallax hero using CSS 3D transforms or Three.js
- Custom particle system — falling petals, stars, fireflies, or snow (canvas-based, no library)
- Micro-interactions on every hover and click
- Custom animated page transitions
- Cinematic video hero support

### RSVP & Guest Data
- RSVP form submissions go to couple's Google Sheet via Apps Script webhook
- Couple views all responses directly in their Google Sheet (their "dashboard")
- RSVP confirmation email to guest via EmailJS
- Couple gets email notification on each RSVP via EmailJS
- CSV export: built into Google Sheets natively
- No third-party RSVP platform — just Google (which they already have)

### Guest Management
- Digital invitation link generation — shareable per-guest URL with pre-filled name
- RSVP tracking via Google Sheets (open, not responded, responded)
- Couple can send reminders by exporting emails from their sheet

### Content
- AI-assisted story writing — couple answers 10 prompts, we generate the story section copy using Claude API (one serverless call, no ongoing dependency)
- Post-wedding gallery update included (one session after the wedding)

### Support
- Dedicated WhatsApp support line
- 2 design revision rounds before launch
- Priority support on the wedding day itself

---

## What's NOT Included
*(Available in lower tiers — Bespoke is the ceiling)*
- Nothing excluded — Bespoke is the full feature set

---

## Tech Stack (Minimal Services)

| Purpose | Tool | Why |
|---|---|---|
| Framework | Next.js 14 (App Router) | Multi-page, server components, API routes |
| Hosting | Vercel | Already used, free for lower tiers, zero new platform |
| Forms / RSVP | EmailJS (client-side SDK) | Zero backend, free 200/mo, just an API key per client |
| Guest data persistence | Google Sheets + Apps Script | Couple already has Google, zero new platform, free forever |
| Particle animations | Custom canvas JS | No library dependency, ~80 lines of code |
| AI story generation | Claude API (one-shot, server action) | Single API call, not an ongoing service |
| Payments | Stripe Payment Links | No checkout code, just a link per tier |
| Domain | Vercel Domains or Namecheap | Setup once per client |

**No Supabase. No Formspree. No Cloudinary. No Twilio.**

Images: direct URLs from client-provided photos (hosted on Vercel static assets or via URL).
Video: YouTube unlisted embed or direct `.mp4` URL provided by client.

---

## Google Sheets Setup (per client)

The "RSVP dashboard" for Bespoke is a pre-formatted Google Sheet with an Apps Script web app that receives POST requests. Steps:

1. Copy the template sheet to client's Google account
2. Deploy the Apps Script as a web app (takes 2 minutes, one-time)
3. Copy the web app URL into `appsettings.js` as `rsvp.sheetsWebhookUrl`
4. Done — all RSVPs now append to their sheet automatically

No ongoing management needed. Client owns their data.

---

## appsettings.js Schema

```js
const config = {

  // ── Meta ───────────────────────────────────────────────────────────────────
  couple: {
    partner1: 'Sofia',
    partner2: 'Alexander',
    hashtagText: '#SofiaAndAlexander2027',
  },

  // ── Event ──────────────────────────────────────────────────────────────────
  event: {
    date:    '2027-06-14',
    time:    '5:00 PM',
    venue:   'The Grand Hall',
    address: '1 Wedding Lane, New York, NY',
    city:    'New York, NY',
    mapUrl:  'https://maps.google.com/?q=...',
  },

  // ── Design ─────────────────────────────────────────────────────────────────
  // Bespoke: design is custom, not preset-based.
  // Colors, fonts, and animations are set per-page in the custom build.

  // ── Hero ───────────────────────────────────────────────────────────────────
  hero: {
    type:            'video',      // 'video' | 'parallax3d' | 'image'
    videoUrl:        'https://...',
    backgroundImage: 'https://...',
    overlayOpacity:  0.45,
    particles:       'petals',     // 'petals' | 'stars' | 'fireflies' | 'snow' | null
  },

  // ── RSVP & Forms ───────────────────────────────────────────────────────────
  rsvp: {
    sheetsWebhookUrl:   'https://script.google.com/macros/s/...',  // Google Apps Script
    emailjs: {
      serviceId:   'service_xxxxx',
      templateId:  'template_xxxxx',
      publicKey:   'xxxxxxxxxxxxxxx',
    },
    maxGuests: 4,
  },

  // ── Password Protection ────────────────────────────────────────────────────
  access: {
    protected:     true,
    passwordHash:  'sha256_hash_of_password',  // hashed client-side
    hint:          'The month we first met',
  },

  // ── Guestbook ──────────────────────────────────────────────────────────────
  guestbook: {
    sheetsWebhookUrl: 'https://script.google.com/macros/s/...',
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_guestbook_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
    approvedEntries: [
      // Manually approved entries shown publicly
      { name: 'Aunt Maria', message: 'Wishing you a lifetime of joy!' },
    ],
  },

  // ── AI Story (generated once at build time, stored as static copy) ─────────
  story: {
    heading:  'Written in the Stars',
    intro:    'Two souls across a crowded room...',
    milestones: [
      { year: '2019', title: 'First Hello', body: '...' },
    ],
    closing: 'And so their greatest adventure begins…',
  },

  // ── Section pages enabled ──────────────────────────────────────────────────
  pages: {
    home:          true,
    story:         true,
    details:       true,
    schedule:      true,
    gallery:       true,
    rsvp:          true,
    registry:      true,
    location:      true,
    accommodation: true,
    music:         true,
    faq:           true,
    guestbook:     true,
    livestream:    false,
  },

  // ... full schedule, gallery, registry, FAQ arrays
};

export default config;
```

---

## File Structure

```
v5.0-bespoke/
  TIER.md
  clients/
    _demo-bespoke/
      appsettings.js      ← demo config
    [client-name]/
      appsettings.js      ← per-client config
  template/
    package.json          ← Next.js 14
    next.config.mjs
    tailwind.config.js
    src/
      app/
        layout.jsx         ← root layout, loads config
        (home)/
          page.jsx
        story/
          page.jsx
        details/
          page.jsx
        schedule/
          page.jsx
        gallery/
          page.jsx
        rsvp/
          page.jsx
        registry/
          page.jsx
        location/
          page.jsx
        accommodation/
          page.jsx
        music/
          page.jsx
        faq/
          page.jsx
        guestbook/
          page.jsx
      components/
        hero/
          VideoHero.jsx
          ParallaxHero.jsx
          ParticleCanvas.jsx
        layout/
          Navbar.jsx
          PageTransition.jsx
        ui/
          FadeIn.jsx
          ScrollReveal.jsx
        forms/
          RsvpForm.jsx      ← uses EmailJS + Google Sheets webhook
          GuestbookForm.jsx ← uses EmailJS + Google Sheets webhook
          MusicForm.jsx     ← uses EmailJS only
        sections/
          StoryTimeline.jsx
          ScheduleTimeline.jsx
          GalleryGrid.jsx
          RegistryLinks.jsx
          LocationMap.jsx
          AccommodationCards.jsx
          FaqAccordion.jsx
          GuestbookDisplay.jsx
      lib/
        config.js           ← reads CLIENT env var, imports appsettings
        emailjs.js          ← EmailJS helper
        sheets.js           ← Google Sheets webhook helper
        particles.js        ← canvas particle engine
        passwordHash.js     ← client-side SHA-256 check
      styles/
        globals.css
        animations.css
```

---

## How to Add a New Client

```bash
# 1. Create client folder
cp -r clients/_demo-bespoke clients/[client-name]

# 2. Edit config
# Fill in appsettings.js with couple details

# 3. Set up Google Sheet (if using RSVP dashboard)
# - Copy template sheet to client's Google account
# - Deploy Apps Script, paste webhook URL into appsettings.js

# 4. Configure EmailJS
# - Create EmailJS account (or use shared account)
# - Create templates for RSVP + Guestbook notifications
# - Paste service/template/public keys into appsettings.js

# 5. Set password
# - Run: node lib/hashPassword.js "chosen-password"
# - Paste hash into appsettings.js

# 6. Develop
CLIENT=client-name npm run dev

# 7. Deploy
CLIENT=client-name npm run build
# → deploy dist/ to Vercel as new project
```

---

## Demo Client

| Field | Value |
|---|---|
| Names | Aria & Sebastian |
| Date | 2027-09-20 |
| Venue | Villa Ephrussi de Rothschild, French Riviera |
| Preset | Custom — deep navy + gold, editorial layout |
| Password | `riviera2027` |
| Particles | Gold stars |
| Hero | Cinematic video (landscape drone footage) |

---

## Client-Facing Description

### Bespoke — *Built for you, only you*

Bespoke is not a template. It is a custom-built digital experience designed around your specific vision, your aesthetic, and your wedding's story — from the ground up.

Every animation, every page, every interaction is crafted to match who you are as a couple. Your guests will not know it's a website. They'll think it's a film.

**What you get:**
- A multi-page website where every section is its own designed experience
- Custom animations — your wedding's mood in motion
- A private dashboard to see RSVPs, headcount, and dietary requirements in real time
- Digital guestbook — guests leave messages your family can read for years
- Password protection — only your invited guests can see it
- Your own domain (e.g. `ariaandsebastian.com`) set up for you
- AI-written story section — we ask you 10 questions; it reads like a novel
- 6 months of hosting, fully managed
- Priority support on your wedding day

**Perfect for:** Couples who want something that has never been seen before.

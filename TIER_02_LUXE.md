# Tier 2 — Luxe

> Premium template-based tier. Bespoke-level features packaged into the scalable config system. The highest-volume revenue driver.

---

## Status
- **Build status:** Not started
- **Codebase:** `v4.0-luxe/` *(to be created in `wedding-websites/`)*
- **Base:** Fork of `v3.0-prestige/` + new features added on top
- **Framework:** React 19 + Vite (same pattern as Prestige and below)

---

## Pricing
- **Price:** $1,997 (fixed)
- **Payment:** 50% upfront via Stripe, 50% on delivery
- **Turnaround:** 48–72 hours from content receipt
- **Client onboarding time:** 4–6 hours

---

## What's Included

### Everything in Prestige, plus:

| Feature | How it's implemented |
|---|---|
| Password protection | Client-side SHA-256 hash check in JS — no backend |
| Ambient particle animation | Custom canvas engine — petals, stars, fireflies, or snow |
| Digital guestbook | EmailJS sends to couple's email; approved entries shown via `appsettings.js` |
| Custom domain setup | Vercel domain config included as a service |
| Mini RSVP summary | EmailJS sends formatted summary email per RSVP; no real-time dashboard |
| RSVP notifications | EmailJS — couple receives a nicely formatted email per submission |
| Post-wedding gallery update | One included session after the wedding |
| 5 exclusive Luxe design styles | `sakuraDrift`, `grandHotel`, `modernCelestial`, `roseGardenGlass`, `editorialLuxe` |
| 10 exclusive Luxe presets | Presets #28–37 |
| 3 exclusive Luxe colour schemes | `velvetCrimson`, `sakuraMist`, `artDecoGold` |

### Sections (all 11 standard + Guestbook)
`hero` · `story` · `details` · `schedule` · `gallery` · `rsvp` · `registry` · `location` · `accommodation` · `music` · `faq` · `guestbook`

### From Prestige (already included)
- Live countdown timer
- Cinematic video hero background
- 3 Prestige-exclusive design styles (auroraGlass, velvetNight, immersive3d)
- 7 Prestige-exclusive presets (#21–27)
- 11 design styles total, 13 colour schemes, 6 font pairings (before Luxe additions)

---

## What's NOT Included
*(Upgrade to Bespoke for these)*
- Multi-page architecture (Luxe is still single-page)
- Real-time RSVP dashboard (Luxe uses email notifications instead)
- Full guest management system (send invites, track opens)
- 3D parallax / GSAP custom animations (Luxe uses Framer Motion only)
- AI story generation
- Live stream embed
- White-label (Luxe has minimal attribution in footer)
- 6-month hosting (Luxe clients host their own or we host for 1 year standard)

---

## Exclusive Luxe Design Styles

| Key | Class | Vibe |
|---|---|---|
| `sakuraDrift` | `style-sakura-drift` | Soft Japanese blooms, falling cherry petals, watercolour washes, blush tones |
| `grandHotel` | `style-grand-hotel` | Art Deco geometry, deep burgundy + gold, ornate dividers, 1920s opulence |
| `modernCelestial` | `style-modern-celestial` | Minimal dark background, gold constellation line art, crisp editorial typography |
| `roseGardenGlass` | `style-rose-garden-glass` | Frosted glass panels over floral photography, ultra-feminine, soft pink + white |
| `editorialLuxe` | `style-editorial-luxe` | High-fashion editorial layout, oversized typography, stark contrast, B&W + gold |

---

## Exclusive Luxe Colour Schemes

| Key | Primary | Background | Vibe |
|---|---|---|---|
| `velvetCrimson` | `#C4485A` | `#0E0810` | Dramatic, romantic, deep red luxury |
| `sakuraMist` | `#E8A0B4` | `#F5EEF0` | Light, feminine, Japanese spring |
| `artDecoGold` | `#C9A000` | `#1A1408` | Bold, geometric, roaring twenties |

---

## Exclusive Luxe Presets (#28–37)

| # | Name | Style | Colour Scheme | Vibe |
|---|---|---|---|---|
| 28 | `sakuraBloom` | sakuraDrift | sakuraMist | Spring garden, delicate, feminine |
| 29 | `cherryBlossom` | sakuraDrift | blush | Romantic Japanese spring |
| 30 | `grandBallroom` | grandHotel | artDecoGold | 1920s black-tie gala |
| 31 | `art DecoNight` | grandHotel | midnight | Dark glamour, Art Deco |
| 32 | `stellarMinimal` | modernCelestial | cosmicGold | Minimal celestial, editorial |
| 33 | `constellationNight` | modernCelestial | auroraBlue | Dark celestial, romantic |
| 34 | `roseGarden` | roseGardenGlass | blush | Garden party, feminine, floral |
| 35 | `frostedRose` | roseGardenGlass | dustyRose | Muted, frosted, elegant |
| 36 | `editorialBlack` | editorialLuxe | midnight | Fashion-forward, B&W, bold |
| 37 | `editorialGold` | editorialLuxe | artDecoGold | Editorial with warm gold accents |

---

## Tech Stack (Minimal Services)

| Purpose | Tool | Notes |
|---|---|---|
| Framework | React 19 + Vite | Same as Prestige — zero new framework to learn |
| Hosting | Vercel | Same account, new project per client |
| RSVP form | EmailJS (client SDK) | Free tier: 200 emails/mo; upgrade to $15/mo if volume grows |
| Guestbook submissions | EmailJS | Same service ID, different template |
| Particle animation | Custom canvas JS | Built into template, ~100 lines, no library |
| Password protection | Client-side SHA-256 | `crypto.subtle.digest` — built into every browser, no library |
| Domain | Vercel Domains | Configured per client |
| Payments | Stripe Payment Link | Static link, no code |

**No Supabase. No Formspree. No Cloudinary.**

---

## Password Protection (Client-Side)

How it works — simple and effective:
1. `appsettings.js` stores `access.passwordHash` (SHA-256 of the password)
2. On the password screen, the visitor's input is hashed with `crypto.subtle.digest`
3. If hashes match → set a `sessionStorage` flag → site unlocks
4. On every page load, check the flag — if not set, show password screen

```js
// In appsettings.js
access: {
  protected: true,
  passwordHash: '9f86d081884c7d659a2feaa0c55ad015...', // SHA-256 of 'blossom2027'
  hint: 'The season we got engaged',
},

// lib/password.js — hashPassword utility
export async function hashPassword(password) {
  const buf = await crypto.subtle.digest('SHA-256', new TextEncoder().encode(password));
  return Array.from(new Uint8Array(buf)).map(b => b.toString(16).padStart(2, '0')).join('');
}
```

No backend. No API. Works offline. Zero cost.

---

## Particle System (Canvas-Based)

Configured via `appsettings.js`:

```js
hero: {
  particles: 'petals',  // 'petals' | 'stars' | 'fireflies' | 'snow' | null
  particleCount: 35,    // optional, defaults per type
  particleColor: null,  // null = auto from colour scheme
},
```

Each type is a preset animation pattern in `src/ui/ParticleCanvas.jsx`. Pure canvas, no library, ~120 lines total.

---

## Guestbook (Email-Based)

No real-time database needed. Works like this:

1. Guest fills in name + message → submits
2. EmailJS sends entry to couple's email (nicely formatted template)
3. Couple replies to us with which messages to display publicly
4. We update `guestbook.approvedEntries` in `appsettings.js` → redeploy (takes ~2 minutes)

For clients who want "live" guestbook display (optional upgrade): add a Google Sheets Apps Script webhook — same pattern as Bespoke.

```js
// appsettings.js
guestbook: {
  emailjs: {
    serviceId:  'service_xxxxx',
    templateId: 'template_guestbook_xxxxx',
    publicKey:  'xxxxxxxxxxxxxxx',
  },
  // Optional: real-time via Google Sheets
  sheetsWebhookUrl: null,  // set to Apps Script URL for live display
  approvedEntries: [
    { name: 'Aunt Maria', message: 'So happy for you both!' },
    { name: 'Tom & Lisa', message: 'Cannot wait to celebrate!' },
  ],
},
```

---

## appsettings.js Schema

```js
const config = {

  preset: 'sakuraBloom',  // one of presets #1–37

  couple: {
    partner1: 'Isabelle',
    partner2: 'Mateo',
  },

  event: {
    date:    '2027-04-12',
    time:    '4:00 PM',
    venue:   'The Sakura Garden Estate',
    address: '12 Blossom Lane, Kyoto',
    city:    'Kyoto, Japan',
    mapUrl:  'https://maps.google.com/?q=...',
  },

  showCountdown: true,

  hero: {
    preHeading:      'Together with their families',
    backgroundImage: 'https://...',
    videoUrl:        'https://...',   // null for image-only
    overlayOpacity:  0.40,
    particles:       'petals',
    particleCount:   40,
  },

  access: {
    protected:    true,
    passwordHash: '9f86d081884c7d659a2feaa0c55ad015...',
    hint:         'The city we first met',
  },

  sectionOrder: [
    'hero', 'story', 'details', 'schedule',
    'gallery', 'rsvp', 'registry', 'location',
    'accommodation', 'music', 'faq', 'guestbook',
  ],
  sections: {
    hero: true, story: true, details: true, schedule: true,
    gallery: true, rsvp: true, registry: true, location: true,
    accommodation: true, music: true, faq: true, guestbook: true,
  },

  rsvp: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_rsvp_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
    maxGuests: 4,
    mealOptions: ['Chicken', 'Fish', 'Vegetarian'],
  },

  guestbook: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_guestbook_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
    sheetsWebhookUrl: null,
    approvedEntries: [],
  },

  music: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_music_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
  },

  // ... story, schedule, gallery, registry, location, accommodation, faq arrays
};

export default config;
```

---

## File Structure

```
v4.0-luxe/
  TIER.md
  clients/
    _demo-luxe/
      appsettings.js
    [client-name]/
      appsettings.js
  template/
    package.json          ← React 19 + Vite
    vite.config.js
    index.html
    src/
      main.jsx
      App.jsx             ← section router, password gate
      config/
        loader.js         ← reads CLIENT env var, imports appsettings
        presets.js        ← all 37 presets + styles + colours
      sections/
        Hero.jsx
        Story.jsx
        Details.jsx
        Schedule.jsx
        Gallery.jsx
        RSVP.jsx
        Registry.jsx
        Location.jsx
        Accommodation.jsx
        Music.jsx
        FAQ.jsx
        Guestbook.jsx     ← NEW in Luxe
      ui/
        FadeIn.jsx
        ParticleCanvas.jsx    ← NEW in Luxe — canvas particle engine
        PasswordGate.jsx      ← NEW in Luxe — client-side password check
        Countdown.jsx
        DividerFloral.jsx
      lib/
        emailjs.js        ← EmailJS helper (replaces Formspree)
        password.js       ← SHA-256 hash utility
        particles.js      ← particle preset configs
      styles/
        index.css
        design-styles/
          glassmorphism.css
          claymorphism.css
          neumorphism.css
          minimalism.css
          liquid-glass.css
          dark-academia.css
          neo-brutalism.css
          botanical.css
          aurora-glass.css      ← from Prestige
          velvet-night.css      ← from Prestige
          immersive-3d.css      ← from Prestige
          sakura-drift.css      ← NEW Luxe
          grand-hotel.css       ← NEW Luxe
          modern-celestial.css  ← NEW Luxe
          rose-garden-glass.css ← NEW Luxe
          editorial-luxe.css    ← NEW Luxe
```

---

## How to Add a New Client

```bash
# 1. Copy demo config
cp -r clients/_demo-luxe clients/[client-name]

# 2. Edit appsettings.js
# - Set couple names, date, venue, preset, sections
# - Set access.passwordHash (run: node lib/hashPassword.js "password")
# - Set emailjs service/template/public keys
# - Fill story, schedule, gallery, faq, etc. arrays

# 3. Dev preview
CLIENT=client-name npm run dev

# 4. Build + Deploy
CLIENT=client-name npm run build
# → dist/ folder → Vercel new project → assign custom domain
```

**Expected time to onboard a new client:** 4–6 hours (including content collection, build, and deployment)

---

## Demo Client

| Field | Value |
|---|---|
| Names | Isabelle & Mateo |
| Date | 2027-04-12 |
| Venue | Sakura Garden Estate, Kyoto |
| Preset | `sakuraBloom` |
| Password | `blossom2027` |
| Particles | Cherry petals |
| Hero | Video background (cherry blossom garden) |

---

## Client-Facing Description

### Luxe — *The ultimate wedding website experience*

Luxe is where luxury meets technology. You get a website that feels alive — petals drifting across the screen as your guests scroll, a private site only your guests can access, a guestbook they can write in, and a real-time view of who's coming.

**What makes Luxe different:**
- **Particles** — falling petals, stars, or fireflies drift across every page
- **Password protection** — only your invited guests can access the site
- **Digital guestbook** — guests leave messages you'll keep forever
- **RSVP notifications** — you get an email the moment someone RSVPs
- **Custom domain** — e.g. `isabelle-and-mateo.com` set up for you
- **Post-wedding gallery** — we update the site with your professional photos after the wedding
- **5 exclusive designs** only available at Luxe: Art Deco, Sakura, Editorial, Celestial, Rose Garden Glass

Everything in Prestige, and then some.

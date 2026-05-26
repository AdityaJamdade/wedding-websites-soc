# Tier 5 — Essential

> Production-ready. The entry-level tier. 5 beautiful sections, full design library, mobile-first. The easiest sell and the fastest to onboard.

---

## Status
- **Build status:** Production ready ✓
- **Codebase:** `wedding-invitation-websites-2.0/` *(shared with Signature — same codebase, fewer sections)*
- **Framework:** React 19 + Vite
- **Demo client:** `clients/_demo-essential/` — Emma & Oliver

---

## Pricing
- **Price:** $149 (fixed)
- **Payment:** Full upfront via Stripe
- **Turnaround:** Same day from content receipt
- **Client onboarding time:** Under 1 hour

---

## What's Included

### Sections (5 only)
`hero` · `story` · `details` · `gallery` · `rsvp`

- **Hero** — couple photo with custom overlay, heading, date, optional pre-heading
- **Our Story** — milestone timeline with dates, titles, and descriptions
- **Wedding Details** — date, time, venue, address, dress code, notes
- **Gallery** — photo grid (typically 4–8 photos)
- **RSVP** — online RSVP form with guest count + meal preferences + message

### Full Design Library (same as Signature)
- 20 standard presets (#1–20)
- 8 design styles
- 10 colour schemes
- 6 font pairings

### Other
- Full mobile responsiveness
- Smooth Framer Motion scroll animations
- Custom couple names, wedding date, venue
- Hero photo with adjustable overlay opacity
- Built-in Formspree RSVP form *(pending EmailJS migration)*

---

## What's NOT Included
*(Upgrade to Signature for these)*
- Countdown timer
- Schedule, Registry, Location, Accommodation, Music Requests, FAQ sections

*(Upgrade to Prestige for these)*
- Video hero, premium design styles, premium presets

---

## Design Options

### Same full design library as Signature

**20 standard presets, 8 design styles, 10 colour schemes, 6 font pairings**
*(See TIER_04_SIGNATURE.md for full list)*

---

## Tech Stack (Minimal Services)

| Purpose | Tool | Notes |
|---|---|---|
| Framework | React 19 + Vite | Shared with Signature |
| Hosting | Vercel | Per-client deployment |
| RSVP form | **EmailJS** (replaces Formspree) | ⚠️ Migration needed (same fix as Signature) |
| Payments | Stripe Payment Link | Static link |

**One EmailJS migration in `RSVP.jsx` fixes both Essential and Signature at the same time.**

---

## appsettings.js Schema

```js
const config = {
  preset: 'romanticBloom',   // any of presets #1–20

  couple: {
    partner1: 'Emma',
    partner2: 'Oliver',
  },

  event: {
    date:    '2027-06-21',
    time:    '3:00 PM',
    venue:   'The Rose Garden',
    address: '42 Garden Lane, Bath, UK',
    city:    'Bath, UK',
    mapUrl:  'https://maps.google.com/?q=...',
  },

  showCountdown: false,   // ← false for Essential; Signature sets true

  hero: {
    preHeading:      'Together with their families',
    backgroundImage: 'https://...',
    overlayOpacity:  0.40,
    // Note: no videoUrl — video hero is Prestige-only
  },

  // Essential: only 5 sections
  sectionOrder: ['hero', 'story', 'details', 'gallery', 'rsvp'],
  sections: {
    hero:    true,
    story:   true,
    details: true,
    gallery: true,
    rsvp:    true,
    // All others false / not listed
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

  story: {
    sectionLabel: 'Our Story',
    heading:      'A Love Story',
    intro:        'It started with a chance meeting...',
    closing:      'And the best chapter is yet to come.',
    milestones: [
      { year: '2022', title: 'How We Met', body: '...' },
      { year: '2023', title: 'The Proposal', body: '...' },
    ],
  },

  details: {
    sectionLabel: 'Wedding Details',
    heading:      'Join Us',
    items: [
      { icon: '🌸', label: 'Date', value: 'June 21st, 2027' },
      { icon: '⏰', label: 'Time', value: '3:00 PM' },
      { icon: '📍', label: 'Venue', value: 'The Rose Garden, Bath' },
      { icon: '👗', label: 'Dress Code', value: 'Garden Smart' },
    ],
    note: 'Parking is available on site.',
  },

  gallery: {
    sectionLabel: 'Our Photos',
    heading:      'Our Favourite Moments',
    photos: [
      { url: 'https://...', alt: 'Engagement photo' },
      // 3–7 more photos
    ],
  },
};

export default config;
```

---

## File Structure (shared with Signature)

```
wedding-invitation-websites-2.0/    ← shared codebase
  clients/
    _demo-essential/
      appsettings.js                ← Emma & Oliver (5 sections, showCountdown false)
    _demo-signature/
      appsettings.js                ← Isabella & James (11 sections, showCountdown true)
    [client-name]/
      appsettings.js
  template/
    (see TIER_04_SIGNATURE.md for full structure)
```

---

## How to Add a New Client

```bash
# 1. Copy demo config
cp -r clients/_demo-essential clients/[client-name]

# 2. Edit appsettings.js
# - Set preset (#1–20), couple, event, hero image URL
# - Fill story milestones, gallery photos, rsvp emailjs keys
# - Keep sectionOrder to ['hero', 'story', 'details', 'gallery', 'rsvp']

# 3. Dev preview
CLIENT=client-name npm run dev

# 4. Build + Deploy (fastest turnaround — can be same-day)
CLIENT=client-name npm run build
# → dist/ → Vercel new project
```

---

## Pending Work
- [ ] Migrate RSVP.jsx from Formspree → EmailJS (same fix as Signature)
- [ ] Consider a "starter" 3-step intake form (name, date, photo upload) to make onboarding even faster

---

## Business Role

Essential serves three purposes:
1. **Entry-point** — lowest barrier to buy, highest volume
2. **Upsell funnel** — couples who see the full 11-section Signature demo often upgrade themselves
3. **Referral driver** — happy Essential clients talk to other couples who may buy at higher tiers

The $149 price is deliberately low to minimise objection and maximise volume. The economics work because onboarding takes under 1 hour.

---

## Demo Client

| Field | Value |
|---|---|
| Names | Emma & Oliver |
| Date | 2027-06-21 |
| Venue | The Rose Garden, Bath, UK |
| Preset | `romanticBloom` |
| Sections | 5 (hero, story, details, gallery, rsvp) |

---

## Client-Facing Description

### Essential — *Your love story, beautifully told online*

Your wedding website, done properly. The Essential plan gives you everything you need to share your big day with your guests — a stunning hero with your photo, your story, all the wedding details, a photo gallery, and an online RSVP form.

Choose from 20 hand-crafted visual presets and 8 distinct design styles so the website feels like it was made for you.

**What your guests get:**
- A beautiful first impression the moment they open the link
- All the details they need: venue, date, time, dress code
- The ability to RSVP online in seconds
- A glimpse into your story through the gallery and timeline

**Perfect for:** Couples who want a clean, modern wedding website without the extras.

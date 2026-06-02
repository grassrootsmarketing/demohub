# Handoff — Gus's Community Market Scheduling Portal (Demohub)

## How to use this doc
Paste this into a new Claude Cowork session along with the file
`gus-community-market-scheduling-portal.html`. It captures the full project
state so work resumes without re-explaining anything.

---

## What this project is
A multi-tenant in-store demo booking platform with two connected products:
1. **Scheduling portals** for independent grocery retailers (single HTML prototype each).
2. **Demohub** — a B2B SaaS marketing one-pager to sell the platform to other independent markets.

**Platform mechanics:** 4-hour non-cancelable demo slots; 80/20 revenue split
(retailer/platform) via Stripe Connect; Outlook/Google Calendar sync for store
managers; no monthly subscription — 20% platform fee model.

**Retailers built so far:** Woodlands Market (portal complete) and Gus's
Community Market (this file). Portals share one template — only content and
branding are swapped per retailer. That shared-template pattern is how new
retailers get added.

---

## The file
`gus-community-market-scheduling-portal.html` — a single self-contained HTML
prototype (frontend only; no backend wired yet). It has three views toggled by
JS `showView()`: **landing** (location picker), **client** (calendar + booking),
and **admin** (dashboard). The five Gus's locations are: Mission Bay, Mission,
Haight St., Noriega, and Canyon (Glen Park).

**Size note:** the file is now ~167KB. It was 944KB until this session — the
eight copies of the logo were collapsed into a single JS constant `GUS_LOGO`,
referenced by every `<img class="gus-logo">` and applied on `DOMContentLoaded`.
Keep it this way; do not re-inline the image eight times.

---

## Done this session
- **Address tooltips** added to all five location cards on the landing page.
  Hovering a location line shows the exact street address in a themed tooltip
  (pure CSS, `pointer-events: none` so it never blocks the Book Now click):
  - Mission Bay → 1101 4th St, San Francisco, CA 94158
  - Mission → 2111 Harrison St, San Francisco, CA 94110
  - Haight St. → 1530 Haight St, San Francisco, CA 94117
  - Noriega → 3701 Noriega St, San Francisco, CA 94122
  - Canyon (Glen Park) → 2815 Diamond St, San Francisco, CA 94131
- **Admin view branding fixed** — header now reads "Gus's Community Market"
  (was still "Woodlands Market" from the template).
- **Location filter added to the admin Upcoming Demos table** — a dropdown in
  the table header (All Locations + the five stores), a new Location column,
  six seeded demo rows spread across all five locations, and an empty-state
  message. Driven by `filterDemos()` reading `data-location` on each row.
- **Logo de-duplication** — 944KB → 167KB (see "The file" above).

---

## Open / next steps
1. **Admin stat cards are still a whole-business rollup.** The top cards
   (This Month's Demos, Revenue 80%, Platform Fee 20%, Demo Fee) do NOT react to
   the location filter. Making them per-location means moving the demo data into
   a JS array as the source of truth and computing the stats + table from it,
   rather than hardcoded values/rows. This is the natural next task.
2. **Deployment to a live, shareable URL.** Still unresolved. Anonymous HTML
   hosts (catbox, tmpfiles, 0x0, uguu) now block `.html` or are offline.
   Vercel/Netlify/Surge need login on the user's own device. Cleanest mobile
   paths: GitHub Gist + htmlpreview.github.io for a quick rendered link, or
   Vercel for a real product URL. Cowork (desktop) makes Netlify Drop / Vercel
   CLI viable.
3. **Demohub marketing site** (`demohub-marketing-site.html`) — built as a
   one-pager, but a working deploy URL was never obtained. Same deploy
   constraints apply.
4. **Backend integrations** (broader scope): Stripe Connect (payments),
   Resend (email confirmations), Vercel (hosting), Outlook + Google Calendar
   (store-manager sync).

---

## Key constraints & conventions
- **Mobile-first.** The user works on mobile. Don't assume drag-and-drop or
  desktop file management without confirming. (Cowork shifts this — desktop
  workflows are now available.)
- **Editing style.** Use targeted `str_replace` edits, not full read-modify-rewrite.
  Inspect with `grep`/`sed`/`wc -l`; never dump the whole file into a reply.
  When scanning, filter out the base64 blob so it doesn't flood output.
- **No hardcoded month/year** in calendar HTML — past bugs came from the
  calendar defaulting to the wrong month. Current JS derives month/year from
  `new Date()`.
- **Communication:** direct and minimal; low tolerance for friction or
  unnecessary workarounds.
- **Tooling that caused context pressure before:** multiple active MCP
  connectors (Day AI, PayPal, Gmail, Google Calendar, Slack). Keep enabled
  connectors lean.

---

## Quick reference — where things live in the file
- `GUS_LOGO` constant + logo loader: top of the `<script>` block.
- View switching: `showView()`.
- Landing location cards (with address tooltips): the `.location-grid` block;
  tooltip CSS is the `.address-tooltip` rules near `.location-card-location`.
- Admin dashboard: `#adminView`; the filter is `#locationFilter`, table body is
  `#demoTableBody`, empty state is `#demoEmptyState`, logic is `filterDemos()`.
- Booking/calendar logic: `renderCalendar()`, `selectDate()`, `selectTime()`,
  `addToCart()`, `confirmBooking()`, plus the ICS/Google/Outlook calendar helpers.

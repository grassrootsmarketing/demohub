# Demohub

A multi-tenant in-store demo booking platform with two connected products:

1. **Scheduling portals** — booking sites for independent grocery retailers (one self-contained HTML prototype each).
2. **Demohub** — a B2B SaaS marketing one-pager that sells the platform to other independent markets.

## Platform model

- 4-hour, non-cancelable demo slots.
- 80/20 revenue split (retailer/platform) via Stripe Connect.
- Outlook / Google Calendar sync for store managers.
- No monthly subscription — the platform earns the 20% fee per demo.

Portals share a single template; only content and branding are swapped per retailer. That shared-template pattern is how new retailers are added.

## What's in this repo

| File | Description |
| --- | --- |
| `gus-community-market-scheduling-portal.html` | Gus's Community Market portal — a single self-contained HTML prototype (frontend only, no backend wired yet). |
| `HANDOFF-gus-portal.md` | Full project-state handoff: what's done, open next steps, conventions, and a map of where things live in the file. |

The portal has three views toggled by the JS `showView()` function:

- **Landing** — location picker (five Gus's locations, with address tooltips).
- **Client** — calendar + booking flow.
- **Admin** — dashboard with an upcoming-demos table and a per-location filter.

## Running it

It's a single static HTML file. Open `gus-community-market-scheduling-portal.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/gus-community-market-scheduling-portal.html
```

## Conventions (carried over from the handoff)

- **Keep the logo de-duplicated.** The file dropped from 944KB to ~167KB by collapsing eight inline logo copies into one JS constant (`GUS_LOGO`) applied on `DOMContentLoaded`. Do not re-inline the image.
- **No hardcoded month/year** in the calendar — month/year derive from `new Date()`.
- **Targeted edits.** Make small, surgical changes rather than rewriting the whole file; filter out the base64 blob when scanning so it doesn't flood output.

## Open / next steps

1. Make the admin stat cards react to the location filter (move demo data into a JS array as the source of truth, compute stats + table from it).
2. Deploy to a live, shareable URL (GitHub Pages, Vercel, or Netlify).
3. Build / deploy the Demohub marketing one-pager.
4. Backend integrations: Stripe Connect (payments), Resend (email confirmations), hosting, Outlook + Google Calendar sync.

See `HANDOFF-gus-portal.md` for the full detail.

# Curl Power — website

Static site. No build step, no framework, no dependencies.

```
index.html   the whole site
logo.png     full lockup, transparent background (hero)
mark.png     icon only, transparent background (header + favicon)
owner.jpg    founder portrait (About section), 1000x1250
```

## Before you go live

Open `index.html` and search for `EDIT ME`. Six things need you:

| What | Where |
|---|---|
| `YOURDOMAIN.com` | three `<head>` tags |
| Opening hours | Visit section |
| Email address | Visit section |
| Instagram handle | Visit section |
| Two About paragraphs | About section |
| Your name in the portrait `alt` text | About section |
| Three client photos | "Recent clients" |

For the photos, drop your image files next to `index.html` and replace each
dashed `<div class="slot">...</div>` block with:

```html
<img src="photo-1.jpg" alt="Curl cut">
```

Portrait orientation, roughly 4:5. Resize them to about 1000px wide before
uploading — straight-from-the-phone photos are several MB each and will make
the page slow on mobile data.

## Brand

Palette is from the Crown Oasis brand board:

| | |
|---|---|
| Deep green | `#2F5D50` |
| Coral | `#FF6F61` (fills) · `#F05A4B` (buttons) · `#B85046` (small text) |
| Gold | `#F4B942` (fills) · `#F6C768` (small text on green) |
| Sand | `#EBDCCB` |
| Off-white | `#F8F6F2` |

The three coral and two gold values exist because the raw brand colors don't
reach WCAG AA contrast at small text sizes. Fills and large display type use
the brand value; small text uses the adjusted one. Every text/background pair
on the site has been checked at 4.5:1 or better.

Type is Cormorant Garamond (headings) and Jost (everything else), both from
Google Fonts.

## Booking

Scheduling still runs entirely on Acuity. The site embeds it:

```html
<iframe src="https://app.acuityscheduling.com/schedule.php?owner=29866814"
        width="100%" height="800" frameborder="0"></iframe>
<script src="https://embed.acuityscheduling.com/js/embed.js"></script>
```

Appointments, clients, intake forms, reminder emails and payments are
unchanged — this is the same calendar, displayed on this domain.

To restyle the calendar so it matches the site, do it inside Acuity:
**Scheduling Page Link & Embedding → Customize Appearance**.

## Deploy (Render static site)

1. Push this folder to a GitHub repo.
2. Render → **New → Static Site** → connect the repo.
3. Build Command: *(leave empty)*
4. Publish Directory: `.`
5. Create. Every push to the default branch redeploys automatically.

## Custom domain (GoDaddy DNS)

In Render: **Settings → Custom Domains → Add**. Add `yourdomain.com`;
Render adds the `www` counterpart with a redirect automatically, then shows
you the exact DNS values to use.

In GoDaddy DNS:

- **Delete** any existing `A` record on `@` (the GoDaddy parking page).
- **Delete** every `AAAA` record. Render is IPv4-only and AAAA records break
  both routing and certificate issuance.
- **Add** `A` · host `@` · value `216.24.57.1` — confirm against your dashboard
- **Add** `CNAME` · host `www` · value `your-service.onrender.com`

GoDaddy also parks new domains with a **Forwarding** rule. If one exists,
turn it off — it silently overrides your A record.

Back in Render, click **Verify**. HTTPS is issued automatically via
Let's Encrypt once DNS resolves — usually minutes, occasionally an hour.

## Optional: a short booking link

Point `book.yourdomain.com` straight at Acuity using GoDaddy's subdomain
forwarding, sending it to `https://app.acuityscheduling.com/schedule/35bbb7fe`.
Useful for texts, business cards and an Instagram bio.

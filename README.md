# Arete Fitness — Website

Multi-page, SEO-optimized static site for Arete Fitness (Lake Forest, CA).

---

## Files

| File | What it is |
|------|-----------|
| `index.html` | Home — hero slideshow, programs, pricing preview |
| `individual-sessions.html` | Session rates ($40 single; others Coming Soon) |
| `bundles.html` | Group discounts, referral, Coming Soon bundles |
| `schedule.html` | Live Google Calendar (month view) |
| `about.html` | Coach Alex bio + portrait |
| `contact.html` | Contact details + map |
| `book.html` | **Booking form** — Formspree connected, NOT indexed |
| `styles.css` | Shared stylesheet |
| `assets/hero-1..3.jpg` | Hero slideshow images |
| `assets/backdrop.jpg` | Fixed background for non-hero pages |
| `assets/coach-alex.jpg` | Coach Alex portrait |
| `sitemap.xml` / `robots.txt` | Search engine files |
| `_redirects` | Clean-URL rules (`/about` → `about.html`) |
| `netlify.toml` | Disables Netlify post-processing — see troubleshooting |

---

## Current pricing (as built)

**Available now**
- Single session — **$40**
- Group of 4 or more — **10% off**
- Group of 6 or more — **20% off**
- Refer a friend — **you get $5 off** your next purchase

**Coming Soon** (shown crossed out, no booking button)
- Intro session ($15), Private 1-on-1 ($60)
- 4 / 8 / 12-session bundles
- Monthly membership

Discounts do not stack.

---

## How booking works

No Stripe, no Calendly. `book.html` posts to Formspree
(`https://formspree.io/f/xqergjdn`) and emails Alex the request.
**Payment is handled in person.**

Buttons pre-fill the session dropdown via a URL parameter:

| Link | Pre-selects |
|------|-------------|
| `book.html?session=single` | Single Session — $40 |
| `book.html?session=group4` | Group Session, 4+ — 10% off |
| `book.html?session=group6` | Group Session, 6+ — 20% off |

Anything unrecognized just leaves the dropdown blank — it fails safe.

The form submits via `fetch` for an inline success message, but also has a
real `action`/`method`, so it still works if JavaScript fails.

---

## Hero slideshow

Three images on a 27-second loop. Each slide pans from its top edge to its
bottom edge over 9 seconds, then crossfades to the next.

- Each layer is 140% of the hero height, which is what creates room to pan.
- `background-size: cover` guarantees the frame is filled at every screen
  size — there is no aspect ratio that produces blank space at the sides.
- Visitors with "reduce motion" enabled see a single still frame.

To swap an image: replace `assets/hero-1.jpg` (or `-2`, `-3`). Keep them
portrait-orientation and roughly 1600px wide.

---

## BEFORE GOING LIVE — required find/replace

**1. Domain.** Search all files for `https://www.aretefitness.com` and replace
with the real domain. It appears in canonical tags, Open Graph tags, the
JSON-LD structured data, `sitemap.xml`, and `robots.txt`.

**2. Demo contact info.** Replace these — they appear in `contact.html` AND in
the JSON-LD on every page. Fake contact info in structured data is worse than
none.
- Phone: `(555) 555-0100`
- Email: `hello@aretefitness.com`

**3. Still placeholder** (all visibly badged on-page):
- Instagram / X links in `contact.html`
- The general contact form in `contact.html` is not connected. (The *booking*
  form is.) Either wire it to a second Formspree form or remove it and point
  people at `book.html`.

---

## Maintenance notes

- **Calendar:** `schedule.html` embeds Alex's real Google Calendar in month
  view. Events added in Google Calendar appear automatically. The calendar
  must stay set to public or the embed will show as empty.
- **Turning on a Coming Soon item:** remove `is-coming-soon` from the card's
  class, delete the `<span class="coming-soon-badge">`, and replace it with a
  button pointing at `book.html?session=...`.
- **Adding a coach:** the About page has a "Room for future coaches" block
  ready to be replaced.

---

## SEO

- Unique title + meta description per page
- Canonical, Open Graph, and Twitter tags
- `ExerciseGym` structured data sitewide; `Service`/`Offer` on rates; `Person`
  on About
- `sitemap.xml` + `robots.txt`; `book.html` excluded from both
- One `<h1>` per page, real `<a href>` links so every page is crawlable

After launch, submit `sitemap.xml` in Google Search Console.

---

## If pages load the wrong content (Netlify)

**Symptom:** URLs serve the wrong file — the homepage shows the contact page,
`/about` downloads an image, `/bundles` shows the booking form, etc.

**Cause:** This is not a problem with the HTML. The links in these pages point
at real filenames (`about.html`, `bundles.html`, …) and are correct. The
breakage happens *after* upload, in Netlify's post-processing layer, which
rewrites links and remaps paths. When that remapping goes wrong, every URL
resolves to the wrong file.

**Fix — do all three:**

1. **Keep `netlify.toml` in the deploy.** It sets `skip_processing = true`,
   which disables the rewriting entirely.
2. **Turn off asset optimization in the dashboard**, in case a previous setting
   is cached: Site configuration → Build & deploy → Post processing → Asset
   optimization → **Disable asset optimization**.
3. **Redeploy from scratch.** Don't upload on top of the broken deploy. Drag
   the whole `arete-fitness-site` **folder** onto Netlify (drag the folder
   itself, not the loose files inside it — dragging many individual files is
   what tends to scramble the mapping).

**How to verify before testing in a browser:** in Netlify, open
Deploys → click the newest deploy → **Files**. You should see `index.html`,
`about.html`, `bundles.html`, an `assets/` folder, and so on, with the right
sizes. If that list looks wrong, the upload failed — redeploy.

**Then hard-refresh** (Ctrl+Shift+R / Cmd+Shift+R) or open the site in a
private window. A normal refresh will often keep serving the broken cached
version.

---

## Clean URLs

The site uses short URLs — `/about` rather than `/about.html` — via the
`_redirects` file.

Those rules use status **200**, which tells Netlify "serve this file" rather
than "bounce the browser somewhere else." Nothing is ever redirected, so there
is no way to create a redirect loop. Both `/about` and `/about.html` resolve to
the same page; the canonical tag in each page points at the short form, so
search engines treat that as the real address.

**`_redirects` must sit in the same folder as `index.html`.** If it goes
missing, the short links break. After any redeploy, click one nav link to
confirm it loads — that is enough to know the file was picked up.

To add a new page: create `newpage.html`, add a line to `_redirects`
(`/newpage  /newpage.html  200`), and link to it as `/newpage`.

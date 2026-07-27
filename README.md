# Arete Fitness — Website

Static multi-page site. No build step, no dependencies. Works when opened
locally, on GitHub Pages, and on Netlify.

---

## Files

| File | What it is |
|------|-----------|
| `index.html` | Home — hero slideshow, programs, pricing preview |
| `individual-sessions.html` | Session rates ($40 single; others Coming Soon) |
| `bundles.html` | Bundles & membership (Coming Soon), group discounts, referral |
| `schedule.html` | Live Google Calendar, month view |
| `about.html` | Coach Alex bio + portrait |
| `contact.html` | Contact details + map |
| `book.html` | Booking form (Formspree) — intentionally not indexed |
| `styles.css` | The only stylesheet |
| `assets/` | `hero-1..3.jpg`, `backdrop.jpg`, `coach-alex.jpg` |
| `sitemap.xml`, `robots.txt` | Search engine files |
| `_redirects`, `netlify.toml` | Netlify-only; harmless elsewhere |
| `.nojekyll` | Stops GitHub Pages from stripping `_redirects` |

**These 12 items are the entire site.** If your repo contains anything else,
delete it (see below).

---

## Putting this in GitHub — read this first

**Delete everything in the repo before copying these files in.** Do not merge
them into what is already there.

Stale files from earlier drafts are almost certainly why you are seeing
problems. Specifically, delete any of these if present:

- an `about.html` that is very large (~86 KB, one enormous line of text) —
  that was a one-off standalone preview with the whole stylesheet and an
  image encoded inline. The real `about.html` here is about 8 KB.
- `arete-fitness.html` — an old single-file version of the entire site
- `arete-setup-instructions.html` — removed from the project
- `assets/hero.jpg` — replaced by `hero-1.jpg`, `hero-2.jpg`, `hero-3.jpg`

**Confirm the homepage is named exactly `index.html`** — lowercase, with the
`.html` extension. GitHub Pages and Netlify both look for that exact name and
will not serve a homepage without it.

After copying the files in, open `index.html` by double-clicking it. If the
hero images and Alex's portrait appear locally, the files are correct and any
remaining problem is on the host, not in the code.

---

## Why images were not loading

Every path in this build is **relative** — `styles.css`, `assets/hero-1.jpg` —
never `/styles.css` or `/assets/hero-1.jpg`.

A leading slash means "start at the very top of the domain." That breaks in
two places:

- **Opening a file locally.** `/assets/hero-1.jpg` becomes
  `file:///assets/hero-1.jpg`, which points at the root of your hard drive.
  Nothing loads.
- **GitHub Pages project sites**, served at `username.github.io/repo-name/`.
  `/assets/hero-1.jpg` looks in `username.github.io/assets/` — the wrong
  place, one level too high.

Relative paths resolve from the page's own folder, so they work in both cases
and on Netlify too. **Keep new image paths relative — no leading slash.**

---

## Links and URLs

Internal links use real filenames (`bundles.html`), which work everywhere with
no configuration.

`_redirects` additionally lets Netlify serve `/bundles` without the extension.
That is a bonus, not a dependency — if the file is ignored (as on GitHub
Pages), every link still works. `.nojekyll` is included because GitHub Pages
otherwise deletes files starting with an underscore.

`netlify.toml` sets `skip_processing = true`. **Leave it in place.** Netlify's
automatic post-processing is what previously scrambled the site so that every
URL served the wrong file.

---

## Current pricing

**Available now**
- Single session — **$40**
- Group of 4+ — **10% off** · Group of 6+ — **20% off**
- Refer a friend — **$5 off** your next purchase

**Coming Soon** (shown at $0 with a badge, no booking button)
Intro session · Private 1-on-1 · 4/8/12-session bundles · Monthly membership

Discounts do not stack.

To activate a Coming Soon item: remove `is-coming-soon` from the card's class,
delete the `<span class="coming-soon-badge">`, restore the real price, and add
a button pointing at `book.html?session=...`.

---

## Booking

No Stripe, no Calendly. `book.html` posts to Formspree
(`https://formspree.io/f/xqergjdn`) and emails Alex. **Payment is in person.**

Buttons pre-fill the dropdown via a URL parameter:

| Link | Pre-selects |
|------|-------------|
| `book.html?session=single` | Single Session — $40 |
| `book.html?session=group4` | Group Session, 4+ — 10% off |
| `book.html?session=group6` | Group Session, 6+ — 20% off |

The form submits via `fetch` for an inline success message, but also has a
real `action`/`method`, so it still works if JavaScript fails.

The `(555) 555-0100` strings inside `book.html` are **input placeholders**
showing the expected phone format — not contact details. Leave them.

---

## Hero slideshow

Three images on a 27-second loop. Each pans from its top edge to its bottom
over 9 seconds, then crossfades.

Each layer is 140% of the hero's height, which is what creates room to pan.
`background-size: cover` guarantees the frame is filled at every screen size,
so no aspect ratio produces blank space at the sides. Visitors with "reduce
motion" enabled see a single still frame.

To swap an image, replace `assets/hero-1.jpg` (or `-2`, `-3`) — keep them
portrait and roughly 1600px wide.

---

## Before launch

**Domain.** Search all files for `https://www.aretefitness.com` and replace
with the real domain. It appears in canonical tags, Open Graph tags, the
JSON-LD, `sitemap.xml`, and `robots.txt`.

**Contact info is already real** — `(510) 586-8332` and
`aretefitness26@gmail.com`, in `contact.html` and in the JSON-LD on every page.

**Still to do:**
- The general contact form on `contact.html` does not send. Either connect it
  to a second Formspree form or remove it and point people at `book.html`.
- `schedule.html` embeds Alex's real Google Calendar. It must stay set to
  public or the embed shows empty.

---

## SEO

Unique title and description per page; canonical, Open Graph and Twitter tags;
`ExerciseGym` structured data sitewide, `Service`/`Offer` on rates, `Person` on
About; one `<h1>` per page; real `<a href>` links so every page is crawlable.
`book.html` is excluded from the sitemap and blocked in `robots.txt`.

After launch, submit `sitemap.xml` in Google Search Console.

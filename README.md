# Arete Fitness — Website

Multi-page, SEO-optimized static site for Arete Fitness (Lake Forest, CA).
Built to be deployed to any static host and indexed by Google.

---

## Files in this repo

| File | What it is | Keep at launch? |
|------|-----------|-----------------|
| `index.html` | Home page | Yes |
| `individual-sessions.html` | Individual session rates | Yes |
| `bundles.html` | Bundles, membership, discounts, referral | Yes |
| `schedule.html` | Google Calendar schedule | Yes |
| `about.html` | About / coach bio | Yes |
| `contact.html` | Contact + map | Yes |
| `styles.css` | Shared stylesheet for all pages | Yes |
| `assets/hero.jpg` | Hero / background image | Yes (replace with owned photo) |
| `sitemap.xml` | Tells Google what pages exist | Yes |
| `robots.txt` | Crawler rules + sitemap pointer | Yes |
| `arete-setup-instructions.html` | **TEMPORARY** setup guide for Alex | **NO — delete** |
| `README.md` | This file | Optional (doesn't affect the live site) |

---

## ⚠️ FILES / EDITS TO REMOVE ONCE EVERYTHING IS FINALIZED

Once payments, booking, calendar, and referrals are all set up and the site is
ready to be public, do these three cleanups:

### 1. DELETE this file from the repo:
```
arete-setup-instructions.html
```
This is the private setup guide. It was only included so Alex could reach it
from a button on the home page. It should never be public.

### 2. REMOVE the temporary setup button from `index.html`
Open `index.html` and delete the entire block that starts with:
```html
<!-- TEMPORARY: Setup-instructions button for Alex. -->
...
<div class="setup-band"> ... </div>
```
It's clearly commented in the file. Deleting it removes the "Website setup
instructions" button between the hero and Programs sections.

### 3. REMOVE the leftover line in `robots.txt`
Delete this line (it points at the now-deleted instructions file):
```
Disallow: /arete-setup-instructions.html
```

That's it. Everything else stays.

---

## 🔧 BEFORE GOING LIVE — required find/replace

The SEO tags use a placeholder domain. Search every file for:
```
https://www.aretefitness.com
```
and replace it with the real domain. It appears in:
- every page's `<link rel="canonical">`, Open Graph, and Twitter tags
- the JSON-LD structured data (`url`, `image`)
- `sitemap.xml`
- `robots.txt`

Also replace the demo contact info everywhere it appears (flagged on-page with
a "demo" badge):
- Phone: `(555) 555-0100`
- Email: `hello@aretefitness.com`
These appear in `contact.html` AND inside the JSON-LD on every page. Fake
contact info in structured data is worse than none — replace before launch.

Other placeholders still to fill (all visibly badged on-page):
- Alex's last name, bio, credentials (`about.html`)
- Real hero photo (`assets/hero.jpg`) — current one is a stand-in, not owned
- Instagram / X links (`contact.html`)
- Contact form backend (Formspree/Basin) — currently shows an alert
- Real Google Calendar source (`schedule.html`) — currently the demo US Holidays calendar

See `arete-setup-instructions.html` for the full step-by-step on all of the above.

---

## SEO notes (what's already done)
- Each page has a unique `<title>` and meta description
- Canonical URLs, Open Graph + Twitter cards on every page
- `LocalBusiness` / `ExerciseGym` JSON-LD structured data on every page
  (so Google can show address, price range, location)
- `Service` + `Offer` structured data on the Individual and Bundles pages
- `sitemap.xml` + `robots.txt` for crawlers
- Semantic HTML headings (one `<h1>` per page), real `<a href>` links so
  every page is independently crawlable and indexable

After launch: submit `sitemap.xml` in Google Search Console to speed up indexing.

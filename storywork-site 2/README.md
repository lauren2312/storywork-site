# StoryWork — storywork.co

Static site for **StoryWork by Lauren Shippy**, deployed on Vercel. Two pages: a homepage and a Business Builder page.

---

## Project structure

```
storywork-site/
├── index.html                       # Homepage at storywork.co
├── businessbuilders/
│   └── index.html                   # Business Builder page at /businessbuilders
├── writing/
│   └── index.html                   # Writing page at /writing — auto-syndicates Substack RSS
├── styles/
│   └── shared.css                   # Single stylesheet for all pages
├── assets/
│   ├── logo.svg                     # StoryWork logo (SWAP with real file)
│   ├── lauren-portrait.jpg          # Portrait (ADD this)
│   ├── book-cover.jpg               # Book cover (ADD this)
│   ├── podcast-cover.jpg            # Podcast art (ADD this)
│   ├── og-image.png                 # Social share image (ADD this)
│   └── README-assets.md             # Asset specs
├── vercel.json                      # Routing + 301 redirects from old URLs
├── robots.txt
├── sitemap.xml
└── README.md                        # This file
```

---

## Before deploy: things to swap in

These are the only swaps that matter. Each one has a `SWAP NOTE` comment in the HTML pointing at the line.

### 1. Logo file
- Replace `assets/logo.svg` with your real logo SVG.
- If your logo is a PNG, name it `logo.svg` won't work — name it `logo.png` and update the two `<img src="/assets/logo.svg">` references in `index.html` and `businessbuilders/index.html` to `logo.png`.

### 2. Portrait photo
- Add `assets/lauren-portrait.jpg` (4:5 ratio, at least 800px wide).
- In `index.html`, find `<!-- SWAP: replace placeholder with...` and replace the `<div class="about-photo">[ Photo of Lauren ]</div>` with:
  ```html
  <div class="about-photo"><img src="/assets/lauren-portrait.jpg" alt="Lauren Shippy"></div>
  ```
- Do the same in `businessbuilders/index.html`.

### 3. Book cover
- Add `assets/book-cover.jpg`.
- In `index.html`, find the book card and replace the `<div class="extra-thumb">Seen,<br>Known<br>+ Heard</div>` with:
  ```html
  <div class="extra-thumb"><img src="/assets/book-cover.jpg" alt="Seen, Known + Heard"></div>
  ```

### 4. Podcast cover
- Add `assets/podcast-cover.jpg`.
- In `index.html`, find the podcast card and replace the `<div class="extra-thumb podcast">&#9658;</div>` with:
  ```html
  <div class="extra-thumb podcast"><img src="/assets/podcast-cover.jpg" alt="Behind the Curtain"></div>
  ```

### 5. Social share image (OG image)
- Add `assets/og-image.png` (1200×630px). Should include logo + tagline.
- The HTML references it already — no edits needed once the file exists.

### 6. Book purchase URL + Podcast URL
- In `index.html`, find the two `SWAP` comments above the book and podcast `<a class="extra-card">` tags.
- Replace `href="#"` with the real URLs.

### 7. Formspree form ID (contact form)
- Sign up at https://formspree.io with `lauren@storywork.co`.
- Create a new form. Free tier covers 50 submissions/month.
- Copy the form ID (looks like `abcd1234`).
- In **both** `index.html` and `businessbuilders/index.html`, find:
  ```
  action="https://formspree.io/f/YOUR_FORM_ID"
  ```
  and replace `YOUR_FORM_ID` with your actual form ID. Both pages should use the same form ID.

That's it. Everything else is wired.

---

## Deploy to Vercel

### Option A: Drag-and-drop (fastest, no Git needed)

1. Zip the entire `storywork-site/` folder.
2. Go to https://vercel.com/new.
3. Drop the zip onto the upload area.
4. Vercel auto-detects this is a static site. Click **Deploy**.
5. You'll get a temporary URL like `storywork-xyz.vercel.app`. Verify both pages render correctly.

### Option B: GitHub-connected (recommended for ongoing edits)

1. Create a new GitHub repo (private or public — your call).
2. Upload all files from `storywork-site/` to the repo root.
3. In Vercel, click **New Project → Import Git Repository**.
4. Pick the repo. Framework preset: **Other**. Root directory: `./`. Click **Deploy**.
5. Future edits: push to GitHub, Vercel auto-deploys.

### Connect storywork.co domain to Vercel

1. In your Vercel project, go to **Settings → Domains**.
2. Add `storywork.co` and `www.storywork.co`.
3. Vercel will show DNS records you need to update. Two ways to handle this:
   - **If your domain registrar is somewhere other than Wix** (e.g. GoDaddy, Namecheap, Google Domains): log in there, update the A record + CNAME as Vercel instructs. Propagation: 5–60 minutes.
   - **If Wix manages your DNS**: in Wix, go to **Domains → storywork.co → Advanced → DNS Records**, and update the A record and CNAME to match Vercel's values. *Note*: this will take storywork.co away from your existing Wix site. Make sure you're ready to fully cut over before doing this.
4. Wait for DNS propagation (Vercel will show a green "Valid" check when it's live).
5. Vercel automatically issues an SSL certificate — your site will be https.

### Redirect laurencshippy.com → storywork.co

The redirect rules in `vercel.json` will automatically 301 any traffic from `laurencshippy.com` (and `www.laurencshippy.com`) to `storywork.co/`. To make this work, you need to add `laurencshippy.com` to the same Vercel project as a second domain.

Steps (after the main storywork.co deploy is live):

1. In your Vercel project, go to **Settings → Domains**.
2. Click **Add Domain**, enter `laurencshippy.com`, click Add.
3. Click **Add Domain** again, enter `www.laurencshippy.com`, click Add.
4. Vercel will show DNS records you need to update at your domain registrar. Two cases:
   - **If laurencshippy.com is currently on Wix or another host**: log into your domain registrar (wherever you bought laurencshippy.com — GoDaddy, Namecheap, Google Domains, etc.) and update the A record + CNAME to match the values Vercel gives you. This will *take laurencshippy.com away from its current host* — make sure you're ready for that.
   - **If laurencshippy.com's DNS is managed at Vercel already**: it's automatic.
5. Wait for DNS propagation (5–60 minutes typically). Vercel will show a green Valid checkmark when the certificate is issued.
6. Test by visiting https://laurencshippy.com — you should be 301-redirected to https://storywork.co/.

What's happening under the hood: both domains point at the same Vercel deployment. The `vercel.json` `redirects` rules detect when the incoming hostname is `laurencshippy.com` (or `www.laurencshippy.com`) and 301-redirect everything to `https://storywork.co/`. Search engines see the 301 and transfer SEO authority from the old domain to the new one over the following weeks.

**Keep the domain registered.** Cost is ~$15/year and prevents anyone else from grabbing it. The redirect runs forever with zero maintenance.

**Want to preserve specific URLs?** If laurencshippy.com has specific pages you want to redirect to specific destinations (e.g., the book page → `/#book`), tell me which URLs and I'll add per-path redirect rules in `vercel.json` so deep links land on the right section of storywork.co instead of the homepage.

### Redirects from old Wix URLs

The `vercel.json` file already includes 301 redirects for these old paths:
- `/strategyservices` → `/businessbuilders`
- `/themarketingengine` → `/`
- `/strategygamemastermind2021` → `/`
- `/team-1` → `/#about`
- `/marketingengineliteworkshop` → `/`
- `/copy-of-home` → `/`
- `/service-page/the-business-builder` → `/businessbuilders`
- Plus common typos for the Business Builder URL (`/business-builder`, `/business-builders`, etc.)

If you have other URLs you want to redirect, add them to the `redirects` array in `vercel.json`.

---

## Post-deploy: SEO + AEO setup (do this within a week of going live)

These steps make sure Google, Bing, and AI search engines (ChatGPT, Perplexity, Google AI Overview) can find and cite the site.

### 1. Submit your sitemap to Google Search Console

1. Go to https://search.google.com/search-console.
2. Click **Add property → URL prefix**, enter `https://storywork.co`, click Continue.
3. Verify ownership. The easiest method is **HTML tag**: Google gives you a meta tag, you paste it into the `<head>` of `index.html`, redeploy, click Verify. (I can add this for you once you have the tag.)
4. Once verified, click **Sitemaps** in the left sidebar.
5. Enter `sitemap.xml` and click **Submit**.
6. Google will start crawling within a few hours. First indexing is usually within 1–7 days.

### 2. Submit your sitemap to Bing Webmaster Tools

Bing matters more than people realize because **ChatGPT's web search runs on Bing's index**. If you want ChatGPT to find and cite your site, Bing indexing matters.

1. Go to https://www.bing.com/webmasters.
2. Click **Add a site**, enter `https://storywork.co`.
3. Easiest path: click **Import from Google Search Console** (uses the verification you already did).
4. Once verified, go to **Sitemaps**, click **Submit**, paste `https://storywork.co/sitemap.xml`.

### 3. Test your structured data

After deploy, paste each URL into https://search.google.com/test/rich-results to confirm Google can parse the Schema.org markup correctly. Test:
- `https://storywork.co/`
- `https://storywork.co/businessbuilders`
- `https://storywork.co/writing`

You should see Person, Organization, ProfessionalService, LocalBusiness on the homepage; Service, FAQ, and Breadcrumbs on the Business Builder page. Any errors get reported there.

### 4. Claim your Google Business Profile

1. Go to https://www.google.com/business.
2. Click **Manage now**, sign in with the Gmail account you want associated with the listing.
3. Search for "StoryWork" — if a listing already exists (it appears it does, given the Google search result you shared), claim it. If not, create it.
4. Category: **Business Consultant** (primary) and **Marketing Consultant** (secondary).
5. Address: Palm Beach Gardens, FL — you can hide the street address if you don't want it public ("I deliver services at customers' locations" option).
6. Service area: list the regions you serve (national is fine).
7. Add the website: `https://storywork.co`.
8. Add a photo: upload your portrait or logo.
9. Verify (Google will mail a postcard or call — usually 5–14 days).

Once verified, your knowledge panel on Google searches for "StoryWork" gets richer (hours, services, reviews, photos), and you become eligible to collect Google reviews — which carry significant weight in both regular search and AI-driven answer engines.

### 5. Set Vercel's free analytics on (optional but helpful)

In your Vercel project: **Settings → Analytics → Enable**. Free tier gives you traffic, top pages, top referrers, Core Web Vitals — no cookie banner required. Good baseline before adding Google Analytics.

---

## Editing the site after launch

Pick the path that matches how you'll work:

- **Quick text edits**: open `index.html` or `businessbuilders/index.html` in any text editor (VS Code, Sublime, even Notepad), edit the text between tags, save, push to GitHub or re-upload to Vercel.
- **Bigger changes**: send me the changes you want and I'll edit and re-export the project.

Common edits and where they live:
| Change | File | Roughly where |
|---|---|---|
| Hero headline / subhead | `index.html` | `<section class="hero">` |
| Bio paragraphs | `index.html` | `<section id="about">` |
| Offering cards | `index.html` | `<section id="offerings">` |
| Business Builder hero | `businessbuilders/index.html` | `<section class="hero">` |
| Scope items | `businessbuilders/index.html` | `<div class="scope-grid">` |
| Footer links | both files | `<footer>` |
| Color palette | `styles/shared.css` | top of file, `:root { ... }` |

---

## Performance & SEO baseline

- Single CSS file, fonts loaded from Google with `preconnect`. Should hit Lighthouse 90+ on performance.
- Real semantic HTML (`<nav>`, `<section>`, `<footer>`, proper headings).
- Open Graph + Twitter Card meta tags on both pages — verify by pasting your live URLs into https://www.opengraph.xyz/.
- Sitemap submitted via `sitemap.xml`. After first deploy, submit it to Google Search Console: **Property → Sitemaps → Add sitemap → `sitemap.xml`**.
- 301 redirects preserve SEO equity from old Wix URLs.

---

## What's intentionally not in this build

These are easy to add later — flag if you want them:

- **Analytics**: no Google Analytics, Plausible, or Fathom installed yet. Vercel has built-in analytics (free tier) you can toggle on with one click in **Settings → Analytics**.
- **Email newsletter signup**: the footer doesn't have a newsletter form. If you want one separate from the contact form, easiest to use a Substack embed or a second Formspree form.
- **Testimonials / client logos**: nothing on the site yet. If you want to add a quote section or client logo strip, easy to add — just need the content.
- **Blog / writing index**: Substack handles your writing, so the site links out instead of hosting posts.
- **Speaker reel / event list**: optional Speaking-specific page if you want one later.

---

## Questions you might have

**Can I edit copy myself in Vercel?**
Vercel doesn't have a CMS-style editor. You edit the HTML files directly (in a text editor or in GitHub's web editor) and push. If you want a CMS layer on top, the cleanest options are TinaCMS or Sanity — but for two pages, that's probably overkill.

**Will my email at storywork.co keep working?**
Yes — email and web are separate DNS records. Vercel only changes web (A records, AAAA, CNAME). Your MX records (email) stay untouched. Just don't blanket-replace your DNS settings.

**What happens to the existing Wix site?**
It stays online but unreachable through storywork.co once DNS is pointed to Vercel. Wix may keep billing you — cancel that subscription once you've verified the new site is live for at least a week.

**Can I A/B test the new copy before going live?**
Yes — deploy to Vercel first as a preview URL (`storywork-xyz.vercel.app`), share that link with people whose feedback you trust, iterate, then connect the domain.

---

Built with care. Edits welcome.

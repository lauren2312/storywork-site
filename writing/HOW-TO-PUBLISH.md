# How to publish a new blog post

This blog lives at `/writing/` on storywork.co. It's hidden from the main nav (only linked in the footer + sitemap) but fully indexable by search engines and AI assistants. Each post is its own folder with its own URL.

---

## The 5-minute publish workflow

### 1. Pick a slug for your post

The slug is the URL-friendly version of your title. Lowercase, hyphens between words, no special characters.

| Title | Slug |
|---|---|
| "Five signs your strategy needs a refresh" | `five-signs-strategy-needs-refresh` |
| "Why we don't run frameworks" | `why-we-dont-run-frameworks` |
| "What I learned co-leading a family office" | `lessons-from-the-family-office` |

Keep it short and keyword-rich. Slugs matter for SEO.

### 2. Copy the existing post folder

In GitHub:

1. Go to your repo → `writing/` folder.
2. Click into `five-signs-strategy-needs-refresh/`.
3. Click into `index.html` → click the pencil/edit icon → **Copy raw file contents** (Cmd+A, Cmd+C).
4. Go back to the `writing/` folder. Click **Add file → Create new file**.
5. Type the path: `writing/your-new-slug/index.html` (the `/` automatically creates the folder).
6. Paste the copied content.

### 3. Update these specific things in the new file

Use Find & Replace to update consistently. You're changing exactly four things:

**A. The post metadata at the top** (search for `POST METADATA`):
- `<title>` — your new headline + " | StoryWork"
- `<meta name="description">` — one-sentence summary (used in Google search results)
- `<meta property="og:url">` — change to `https://storywork.co/writing/your-new-slug`
- `<meta property="og:title">` — your headline
- `<meta property="og:description">` — same description as above
- `<meta property="article:published_time">` — today's date in `YYYY-MM-DD` format
- `<link rel="canonical">` — same URL as og:url

**B. The Schema.org block** (search for `SCHEMA.ORG`):
- `headline` — your headline
- `description` — same as meta description
- `datePublished` — today's date in `YYYY-MM-DD`
- `dateModified` — same as datePublished
- `mainEntityOfPage @id` — your full URL

**C. The article body** (search for `ARTICLE`):
- The eyebrow date/category line
- The h1 headline
- The lead paragraph (italic, in the box)
- The body content — replace with your post

You can use `<h2>`, `<h3>`, `<p>`, `<ul>`, `<ol>`, `<li>`, `<blockquote>`, `<a href="">`, `<strong>`, `<em>`, and `<hr>` tags. The styles from `shared.css` will format everything automatically.

**D. The CTA at the bottom** (search for `post-cta`):
- Update if you want a different call-to-action for this specific post. The default sends people to Business Builder + the contact form, which works for most posts.

### 4. Add a card to the writing index

Open `writing/index.html` in GitHub → pencil/edit icon. Find the section labeled `POST CARDS`. Copy an existing `<a class="blog-card">` block and add it to the **top** of the list (most recent first). Update:

- The href to your new slug folder: `your-new-slug/`
- The date and category in the eyebrow
- The h3 headline
- The p excerpt (1-2 sentences pulling readers in)

### 5. Add the new URL to sitemap.xml

Open `sitemap.xml` in GitHub → pencil/edit icon. Add this block (before `</urlset>`):

```xml
<url>
  <loc>https://storywork.co/writing/your-new-slug</loc>
  <changefreq>monthly</changefreq>
  <priority>0.6</priority>
</url>
```

### 6. Commit

GitHub auto-prompts you to commit each file as you edit. Use a commit message like `Publish: Five signs your strategy needs a refresh`. Vercel auto-deploys in ~30 seconds.

### 7. Push faster indexing (optional)

In Google Search Console → URL Inspection → paste the new post URL → **Request Indexing**. Tells Google to crawl now instead of waiting for the next scheduled crawl.

---

## Scheduling posts

This is a static site, so there's no built-in scheduling like WordPress or Substack. Three ways to handle scheduling:

**Option 1 (simplest): Just commit when ready.** Write the post in advance in a Google Doc or markdown editor. When the publish date arrives, do the GitHub workflow above. Takes 5 minutes.

**Option 2: Use a draft branch.** In GitHub, create a branch called `drafts/your-post-slug`. Build the post on that branch. When ready to publish, merge it into `main`. Slightly more technical but lets you preview the post on a Vercel preview URL before going live.

**Option 3: Schedule with GitHub Actions.** A scheduled GitHub Action can commit a file at a specific time. This is real engineering work — I'd skip unless you're publishing 4+ times per month.

**Recommendation:** Option 1 for now. You'll publish so infrequently in the first few months that the manual step is barely a step.

---

## What posts work best for SEO and AEO

Targets that combine "what your buyers are searching" with "what AI engines cite":

**SEO-friendly title structures:**
- "Five signs [problem your ICP has]"
- "How to know if [decision your ICP is wrestling with]"
- "What I learned [doing the thing you do]"
- "When to [action] vs. when to [other action]"

**Examples in your space:**
- "Five signs your business strategy has outgrown its plan" (already published — your launch post)
- "When to refresh your positioning vs. when to refresh your business model"
- "How to know if your strategy needs a rebuild or just a refresh"
- "What I learned co-leading a family investment office"
- "The strategy questions every 5-year-old company should ask"
- "Why most strategy decks die in execution — and what to do about it"

**Cadence:** Even 1 post per month, consistently, is better than 4 posts in one month then nothing for 6 months.

---

## How AI engines see your blog

Each post has:

- **BlogPosting schema** — tells search engines and AI exactly who wrote it, when, what it's about
- **Speakable schema** — tells AI which sentences to read aloud as a summary
- **Open Graph meta tags** — shows preview cards when shared on LinkedIn, iMessage, etc.
- **Canonical URL** — tells search engines this is the source of truth (not a duplicate)
- **Sitemap inclusion** — tells search engines the post exists

You don't need to do anything extra to get the AEO benefit. Just publish good content using the template, and the technical SEO/AEO is handled.

---

## When in doubt

Replace one post folder, replace the content, commit. Worst case: a typo. You can always edit again — just bump the `dateModified` field in the schema when you do.

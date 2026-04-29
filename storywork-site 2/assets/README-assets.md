# Assets folder — what to swap in

This folder ships with placeholder assets so the site renders cleanly. Replace each placeholder with your actual file using the same filename, and the site will pick it up automatically.

## Files to swap

| Filename | What it is | Notes |
|---|---|---|
| `logo.svg` | StoryWork logo (header + footer) | Currently a typographic approximation. Replace with your actual logo SVG (or PNG named `logo.svg` won't work — use SVG). For PNG, rename to `logo.png` and update the `<img src>` references in both `index.html` files. |
| `lauren-portrait.jpg` | Photo of Lauren for the About section | 4:5 ratio (portrait). At least 800px wide. |
| `book-cover.jpg` | Cover of *Seen, Known + Heard* | ~110×140px display, but upload at least 400×508px so it stays sharp on retina. |
| `podcast-cover.jpg` | Cover art for *Behind the Curtain* | Square format. At least 400×400px. |
| `og-image.png` | Social-share preview image (LinkedIn, iMessage, etc.) | 1200×630px. Should include logo + tagline. |
| `favicon.ico` | Browser tab icon | 32×32 or 48×48. Can also use `favicon.svg` for sharper rendering. |

## Adding new assets

If you want to add additional images (e.g. testimonial photos, client logos), drop them in this folder and reference them as `/assets/your-file.jpg` from the HTML.

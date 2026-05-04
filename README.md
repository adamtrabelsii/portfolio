# Adam Trabelsi — Portfolio

Personal portfolio site for Adam Trabelsi, Full-Stack Developer based in Madrid.

## Run locally

Open `index.html` directly in any browser, or serve with:

```bash
npx serve .
```

Then visit `http://localhost:3000`.

## Deploy to Vercel

**Option A — CLI:**
```bash
npm i -g vercel
vercel
```

**Option B — GitHub import:**
1. Push this folder to a GitHub repository
2. Go to [vercel.com](https://vercel.com) → New Project → Import from GitHub
3. Select the repo — no build config needed, just deploy

The `vercel.json` file handles clean URLs, caching headers, and the 404 page automatically.

## Update content

All content lives in `index.html`. Find sections by their HTML comments:

| Section | Comment in index.html |
|---|---|
| Hero | `<!-- HERO -->` |
| About | `<!-- ABOUT -->` |
| Skills | `<!-- SKILLS -->` |
| Projects | `<!-- PROJECTS -->` |
| Experience | `<!-- EXPERIENCE -->` |
| Education | `<!-- EDUCATION -->` |
| Contact | `<!-- CONTACT -->` |

## Update CV

Replace `uploads/cv-english.pdf` with the new file (keep the same filename).

## Update photo

Replace `uploads/pro-pic.jpg` with a new photo (keep the same filename, portrait orientation works best).

For best performance, also generate a WebP version:
```bash
npx sharp-cli --input uploads/pro-pic.jpg --output uploads/pro-pic.webp
```

## Update domain

When you buy a domain, replace all occurrences of `https://adamtrabelsi.com` in:
- `index.html` (canonical, og:url, og:image, JSON-LD)
- `sitemap.xml`
- `robots.txt`

Then convert `og-image.svg` to `og-image.png` at 1200×630 for social sharing:
```bash
# Using ImageMagick:
magick og-image.svg -resize 1200x630 og-image.png

# Or using rsvg-convert:
rsvg-convert og-image.svg -w 1200 -h 630 -o og-image.png
```

## Add real projects

Find `<!-- PROJECTS -->` in `index.html` and replace the three placeholder `.project-card` blocks with your real projects. See `SONNET_PROMPTS.md` Prompt 6 for the exact format.

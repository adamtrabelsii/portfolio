# Portfolio Project Analysis

## What's Missing

### Content gaps
- 3 project cards are dummies (`Project Name`, placeholder descriptions, `#` links)
- No favicon — browser tab shows default icon
- No `og:image` for link previews (LinkedIn/WhatsApp/Twitter shares look bare)
- GitHub/LinkedIn handles (`adamtrabelsii`) unverified
- No real project screenshots — thumbnails are striped placeholders

### Technical gaps
- No mobile menu — on `<768px` the entire nav-links list is just `display: none` (no hamburger)
- No `prefers-reduced-motion` support — accessibility regression for users with vestibular disorders
- No image optimization — `pro pic.jpeg` is 224KB, served at 320×380 (could be ~30KB as WebP)
- No `loading="lazy"` on the hero photo
- Asset filenames have spaces (`pro pic.jpeg`, `CV English.pdf`) — works but URL-encodes ugly
- No skip-to-content link for screen readers
- No canonical URL / Twitter card meta / JSON-LD Person schema
- No deployment config (no `vercel.json` / `netlify.toml` / GitHub Pages workflow)
- No `README.md` describing how to run/deploy

### Removed-but-useful
- Light/dark mode toggle was in the prototype's tweaks panel — stripped during cleanup. A subset (just the theme toggle) is worth keeping for users.

---

## Recommendations (priority order)

1. **Deploy it.** Vercel/Netlify/GitHub Pages — even an empty portfolio at a real URL beats a local file. Put the URL on your CV.
2. **Replace placeholder projects.** Even 1 real project beats 3 fake ones. Pick the strongest from your GitHub.
3. **Rename assets** — `pro-pic.jpg` and `cv-english.pdf` (kebab-case, no spaces).
4. **Add favicon + og:image** — 1200×630 social card matters for job hunting on LinkedIn.
5. **Mobile hamburger** — without it, mobile users have no navigation.
6. **Optimize the photo** — convert to WebP, add a fallback JPEG, use `<picture>`.
7. **Verify the social handles** before publishing.
8. **Add JSON-LD Person schema** — helps Google show a knowledge panel for "Adam Trabelsi developer Madrid".

---

## Sonnet Prompts to Finish This Off

Drop these into a fresh Claude Code session, one at a time.

### Prompt 1 — Deployment + housekeeping

```
In c:\Users\USER\Desktop\PORTFOLIO, prepare this static portfolio for
deployment:
1. Rename "pro pic.jpeg" → "pro-pic.jpg" and "CV English.pdf" → "cv-english.pdf",
   updating the references in portfolio.html.
2. Rename portfolio.html → index.html so it's served at the site root.
3. Add a favicon.svg (a stylized "AT" mark in the amber accent color) and link it.
4. Add a vercel.json with clean URLs and proper cache headers for /uploads.
5. Add a README.md explaining how to deploy to Vercel and how to update content.
```

### Prompt 2 — SEO + social sharing

```
Add SEO polish to index.html:
1. Generate a 1200×630 og-image.svg (dark bg, "Adam Trabelsi / Full-Stack
   Developer / Madrid" in DM Sans, amber accent on the name).
2. Add og:image, og:url, og:type, twitter:card meta tags.
3. Add a canonical link tag (placeholder URL https://adamtrabelsi.com).
4. Add a JSON-LD <script type="application/ld+json"> Person schema with
   jobTitle, address (Madrid, Spain), email, sameAs (LinkedIn + GitHub),
   knowsLanguage (ar, en, fr, es).
5. Add a sitemap.xml and robots.txt.
```

### Prompt 3 — Accessibility + performance

```
Improve a11y and performance in index.html:
1. Add a "skip to main content" link visible on focus.
2. Wrap main page content in <main id="main"> and the hero/sections accordingly.
3. Respect prefers-reduced-motion: disable scroll-reveal opacity/transform
   transitions and the language-bar animation when set.
4. Add loading="lazy" + decoding="async" + width/height to .hero-photo.
5. Convert pro-pic.jpg to WebP and serve via <picture> with JPEG fallback.
6. Add aria-current="page" handling to the nav as you scroll between sections
   (IntersectionObserver on each section).
```

### Prompt 4 — Mobile menu + theme toggle

```
In index.html add two interactive features without breaking the existing design:
1. A hamburger button (visible only <=768px) that opens a full-screen overlay
   menu mirroring the desktop nav-links + Get-in-touch CTA. Close on link
   click and on Escape. Trap focus while open.
2. A small theme toggle button in the nav (sun/moon icon) that switches
   between the existing dark palette and the light palette
   (#fafaf8/#f0efeb/#ffffff/#111111/#666660 for bg/bg2/surface/text/muted).
   Persist choice in localStorage; respect prefers-color-scheme on first visit.
```

### Prompt 5 — Real projects (run when you have content)

```
Replace the 3 placeholder cards in the #projects section of index.html with
these real projects. For each: title, 1-2 sentence description, tech tags,
GitHub URL, live URL (or omit if none), and a screenshot at
/projects/<slug>.png shown in the .project-thumb instead of the striped
placeholder.

Project 1: <name, description, tech, links>
Project 2: <name, description, tech, links>
Project 3: <name, description, tech, links>
```

---

## How to Perfect It

The portfolio is **visually finished**. Perfection from here is about three things:

1. **Make it real** — deploy it, verify the handles, replace dummy projects. A polished design with `href="#"` links signals "unfinished" to recruiters faster than any styling will save.
2. **Make it findable** — JSON-LD + og:image + canonical + a real domain. Recruiters Google candidates; this is what makes the result look credible.
3. **Make it inclusive** — mobile menu, reduced-motion, lazy loading. Right now ~50% of recruiter traffic (mobile) gets a degraded experience.

Run prompts 1 → 2 → 3 → 4 sequentially; run prompt 5 whenever you've got real content to drop in. After that, the only remaining "perfection" is iterative — adding case studies, blog posts, or a testimonials section as you accumulate them.

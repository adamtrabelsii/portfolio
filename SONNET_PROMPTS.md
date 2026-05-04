# Sonnet Prompts — Portfolio Build-Out

Copy-paste each prompt below into a fresh Claude Code (Sonnet) session, in order.
Each prompt is self-contained — Sonnet starts cold, so the prompt tells it
everything it needs.

Project root: `c:\Users\USER\Desktop\PORTFOLIO`
Current entry file: `portfolio.html`
Assets: `uploads/pro pic.jpeg`, `uploads/CV English.pdf`

---

## What Sonnet Can Do Autonomously

| # | Task | Needs user input? |
|---|------|-------------------|
| 1 | Clean filenames + rename to index.html + favicon + deployment config + README | No |
| 2 | SEO meta tags + og-image + JSON-LD + sitemap + robots | No (uses placeholder domain) |
| 3 | Accessibility + performance (skip link, reduced-motion, lazy loading, aria-current) | No |
| 4 | Mobile hamburger menu + light/dark theme toggle | No |
| 5 | 404 page + print stylesheet for the CV-as-page fallback | No |
| 6 | Replace placeholder projects with real ones | **Yes** — needs project info |
| 7 | Verify domain + replace placeholder URLs | **Yes** — once domain bought |

Run 1 → 5 sequentially. Run 6 when you have real project content. Run 7 last.

---

## Prompt 1 — Housekeeping, deployment, favicon

```
You are working in c:\Users\USER\Desktop\PORTFOLIO. The site is a single
static portfolio.html with assets in uploads/. Make it production-ready:

1. Rename:
   - portfolio.html → index.html (so it serves at site root)
   - uploads/pro pic.jpeg → uploads/pro-pic.jpg
   - uploads/CV English.pdf → uploads/cv-english.pdf
   Update every reference inside index.html to match. The CV download
   attribute should keep the filename "Adam_Trabelsi_CV.pdf".

2. Create favicon.svg in the project root: a 32x32 viewBox SVG with a
   rounded square in the amber accent color (oklch(75% 0.18 41) /
   approximately #d97757) containing the letters "AT" in white DM Sans
   bold, centered. Link it in the <head> with both:
     <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
     <link rel="alternate icon" href="/favicon.ico" />
   (Skip the .ico — modern browsers will fall back to the SVG.)

3. Create vercel.json:
   - cleanUrls: true
   - trailingSlash: false
   - headers: cache-control "public, max-age=31536000, immutable" for
     /uploads/* and /favicon.svg
   - headers: cache-control "public, max-age=0, must-revalidate" for
     /index.html

4. Create README.md with these sections:
   - One-line description
   - "Run locally": just open index.html in a browser, or `npx serve .`
   - "Deploy to Vercel": `npm i -g vercel && vercel` (or via GitHub import)
   - "Update content": which sections live where in index.html
   - "Update CV": replace uploads/cv-english.pdf
   - "Update photo": replace uploads/pro-pic.jpg

Do NOT add a build step, package.json, or any framework. This is a single
static HTML file and should stay that way.

Verify by opening index.html in the browser and clicking through every link.
```

---

## Prompt 2 — SEO + social sharing

```
You are working on c:\Users\USER\Desktop\PORTFOLIO\index.html (a single
static file). Add SEO polish without changing the visual design:

1. In <head>, after the existing <title>, add these meta tags. Use
   "https://adamtrabelsi.com" as the canonical URL placeholder — the user
   will replace this when they buy a domain:

   - <link rel="canonical" href="https://adamtrabelsi.com/" />
   - <meta name="author" content="Adam Trabelsi" />
   - <meta name="keywords" content="full-stack developer, Node.js,
     TypeScript, Madrid, React, Angular, Laravel" />
   - og:title, og:description (already in the source — extract values),
     og:type=website, og:url, og:image=https://adamtrabelsi.com/og-image.png,
     og:image:width=1200, og:image:height=630, og:locale=en_US
   - twitter:card=summary_large_image, twitter:title, twitter:description,
     twitter:image

2. Create og-image.svg in the project root, 1200x630 viewBox:
   - Background: #0b0b0d with the same grid pattern used in the hero
     (60px lines in rgba(255,255,255,0.07))
   - A radial amber glow (oklch(75% 0.18 41 / 0.2)) on the right side
   - Left side: "Adam" in white 110px DM Sans bold, "Trabelsi" below in
     amber, then "Full-Stack Developer · Madrid" in 28px DM Mono muted
   - Bottom-left: a small "adamtrabelsi.com" in DM Mono accent color
   Save as og-image.svg. Then create a README note that the user should
   render it to og-image.png at 1200x630 (most browsers won't use SVG for
   og:image) — provide the exact ImageMagick or rsvg-convert command.

3. Add a JSON-LD Person schema right before </head>:
   <script type="application/ld+json">
   {
     "@context": "https://schema.org",
     "@type": "Person",
     "name": "Adam Trabelsi",
     "givenName": "Adam",
     "familyName": "Trabelsi",
     "jobTitle": "Full-Stack Developer",
     "email": "mailto:ademtrabelsi2001@gmail.com",
     "telephone": "+34633454968",
     "url": "https://adamtrabelsi.com",
     "image": "https://adamtrabelsi.com/uploads/pro-pic.jpg",
     "address": { "@type": "PostalAddress",
                  "addressLocality": "Madrid", "addressCountry": "ES" },
     "nationality": "Tunisian",
     "sameAs": [ "https://linkedin.com/in/adamtrabelsii",
                 "https://github.com/adamtrabelsii" ],
     "knowsLanguage": ["Arabic", "English", "French", "Spanish"],
     "alumniOf": { "@type": "CollegeOrUniversity",
                   "name": "Higher Institute of Arts and Multimedia Manouba" }
   }
   </script>

4. Create sitemap.xml at the project root, listing only "/" with lastmod
   set to today's date and changefreq=monthly.

5. Create robots.txt:
   User-agent: *
   Allow: /
   Sitemap: https://adamtrabelsi.com/sitemap.xml

Validate the JSON-LD by running `node -e "JSON.parse(require('fs').readFileSync('index.html','utf8').match(/<script type=\"application\\/ld\\+json\">([\\s\\S]*?)<\\/script>/)[1])"` and confirm no error. Done.
```

---

## Prompt 3 — Accessibility + performance

```
Working in c:\Users\USER\Desktop\PORTFOLIO\index.html. Improve accessibility
and load performance without altering the visible design.

1. SKIP LINK
   - As the very first child of <body>, add:
     <a class="skip-link" href="#main">Skip to main content</a>
   - CSS: .skip-link is absolutely positioned off-screen (top: -100px),
     becomes visible on :focus at top: 8px left: 8px with the accent
     background, black text, 8px padding, 4px border-radius, z-index: 200.

2. SEMANTIC LANDMARKS
   - Wrap everything from the hero through the contact section in
     <main id="main">…</main>. Keep <nav> and <footer> outside <main>.
   - Confirm there is exactly one <h1> on the page (the hero name).

3. REDUCED MOTION
   Append at the end of the <style> block:

     @media (prefers-reduced-motion: reduce) {
       *, *::before, *::after {
         animation-duration: 0.001ms !important;
         animation-iteration-count: 1 !important;
         transition-duration: 0.001ms !important;
         scroll-behavior: auto !important;
       }
       .scroll-reveal { opacity: 1 !important; transform: none !important; }
       .lang-bar-fill { transition: none !important; }
     }

4. IMAGE PERFORMANCE
   On the hero photo <img>, add:
     loading="eager" fetchpriority="high"
     width="320" height="380"
   (Eager + high priority because it's above the fold; width/height prevents
   layout shift.)

   Wrap it in a <picture> element. The user's photo is currently a JPEG.
   Generate a WebP version: use the `sharp` package via npx if available,
   otherwise leave a TODO comment in the HTML pointing to where the WebP
   <source> goes. Final markup:

     <picture>
       <source srcset="uploads/pro-pic.webp" type="image/webp" />
       <img src="uploads/pro-pic.jpg" ... />
     </picture>

5. NAV SCROLL-SPY
   Replace the existing nav-padding scroll handler with one that ALSO sets
   aria-current="page" on the nav-links anchor whose section is currently
   most-visible (use IntersectionObserver with rootMargin "-40% 0px -55% 0px"
   so a section is "current" only when crossing the middle of the viewport).
   Style: nav-links a[aria-current="page"] { color: var(--accent); }

6. FOCUS STYLES
   Append:

     :focus-visible {
       outline: 2px solid var(--accent);
       outline-offset: 3px;
       border-radius: 2px;
     }
     a:focus:not(:focus-visible) { outline: none; }

7. ARIA POLISH
   - The <ul class="nav-links"> should have aria-label="Primary".
   - The hero scroll hint and location are decorative — add aria-hidden="true"
     so screen readers skip them.
   - Each .skill-chip is decorative, not interactive. They don't need ARIA
     but confirm they are <span>, not <button>.

Test by tabbing through the page from the URL bar — every interactive
element should have a visible focus ring, and the skip link should appear
on first Tab. Done.
```

---

## Prompt 4 — Mobile menu + light/dark theme toggle

```
Working in c:\Users\USER\Desktop\PORTFOLIO\index.html. Add two interactive
features. Keep the existing visual design intact at >=769px.

=== A. MOBILE HAMBURGER MENU ===

Currently `.nav-links { display: none }` at <=768px, leaving mobile users
with no navigation. Replace with a real menu:

1. Add a hamburger button inside <nav>, before the .nav-cta:
     <button class="nav-burger" aria-label="Open menu" aria-expanded="false"
             aria-controls="mobile-menu">
       <span></span><span></span><span></span>
     </button>
   Three spans = three lines. Hidden at >768px (display: none), visible
   below. 28px wide, transparent background, 8px padding.

2. Add a full-screen overlay menu (also hidden at >768px):
     <div id="mobile-menu" class="mobile-menu" aria-hidden="true">
       …copy of nav-links plus the Get-in-touch CTA…
     </div>
   Style: position: fixed, inset: 0, background: var(--bg) with 0.98 alpha,
   backdrop-filter: blur(20px), z-index: 99 (under the nav).
   Display: flex, flex-direction: column, align-items: center,
   justify-content: center, gap: 32px. Links are 22px DM Sans, uppercase,
   muted color, with the same hover→text transition as desktop.
   Initial state: opacity 0, pointer-events: none, transform: translateY(-12px).
   When body has class .menu-open: opacity 1, pointer-events: auto,
   transform: none, transition 0.25s ease.

3. JS:
   - Click burger → toggle body.menu-open, flip aria-expanded and
     aria-hidden, lock body scroll (body.style.overflow='hidden').
   - Click any link inside #mobile-menu → close (via the same toggle path,
     and unlock scroll).
   - Escape key → close.
   - Window resize past 768px → force close.
   - Animate the three burger spans into an X when open: rotate top +45deg,
     hide middle, rotate bottom -45deg, with transform-origin: center.

4. Trap focus inside #mobile-menu while open: on open, focus the first link;
   on Tab from the last link cycle to the first (and Shift+Tab from first to
   last). Use a small keydown handler scoped to #mobile-menu.

=== B. THEME TOGGLE ===

Add a sun/moon button in <nav>, before .nav-cta (and before the burger on
mobile):

  <button class="nav-theme" aria-label="Toggle theme">
    <svg class="icon-sun" …></svg>
    <svg class="icon-moon" …></svg>
  </button>

Use simple Lucide-style line icons inline (no external library). The visible
icon depends on body.theme-light: sun shown in light mode, moon in dark.

Light palette (apply via body.theme-light overrides on :root):
  --bg: #fafaf8;  --bg2: #f0efeb;  --bg3: #e8e7e2;  --surface: #ffffff;
  --border: rgba(0,0,0,0.08);  --text: #111111;  --muted: #555550;
  Keep --accent unchanged so brand identity stays consistent.

Also tweak in light mode:
  - body::before grain opacity: 0.2 (less noisy on light bg)
  - nav background: rgba(250,250,248,0.7)
  - .hero-photo filter: grayscale(0%)  (full color on light)

JS behavior:
  - On load: check localStorage('portfolio-theme'). If 'light' → add
    .theme-light to body. If null → check
    window.matchMedia('(prefers-color-scheme: light)').matches and apply.
  - On click: toggle .theme-light, write the chosen value to localStorage.
  - Add a <meta name="theme-color"> tag whose content updates with the
    theme (#0b0b0d for dark, #fafaf8 for light) so mobile browser chrome
    matches.

Test on a real phone-width viewport: the burger should open a clean
overlay, links should work, the theme button should persist across reloads,
and tabbing through the open menu should not escape it. Done.
```

---

## Prompt 5 — 404 page + print stylesheet

```
Working in c:\Users\USER\Desktop\PORTFOLIO. Two small additions:

1. Create 404.html in the project root. It shares the same head (fonts,
   favicon, base CSS variables) as index.html — extract them into the file
   inline (don't introduce a shared partials system, this is a static site).
   Body content:

     <main style="min-height: 100vh; display: flex; flex-direction: column;
                  align-items: center; justify-content: center;
                  text-align: center; padding: 48px;">
       <div style="font-family: var(--mono); color: var(--accent);
                   letter-spacing: 0.2em; margin-bottom: 16px;">404</div>
       <h1 style="font-size: clamp(48px, 8vw, 96px); font-weight: 700;
                  letter-spacing: -0.03em; margin-bottom: 16px;">
         Page not found
       </h1>
       <p style="color: var(--muted); max-width: 420px; margin-bottom: 32px;">
         This page doesn't exist or has moved. Let's get you back home.
       </p>
       <a href="/" class="btn-primary">← Back to portfolio</a>
     </main>

   Reuse the .btn-primary class definition from index.html.
   Update vercel.json: { "errorPage": "404.html" } or use the rewrites
   convention Vercel expects.

2. Add a print stylesheet inside index.html (at the end of the existing
   <style> block):

     @media print {
       nav, footer, .hero-scroll-hint, .hero-location, .hero-glow,
       .hero-bg-grid, body::before { display: none !important; }
       body { background: white !important; color: black !important; }
       * { color: black !important; background: transparent !important;
           border-color: #ccc !important; box-shadow: none !important; }
       a { color: black !important; text-decoration: underline; }
       a[href^="http"]::after { content: " (" attr(href) ")";
                                 font-size: 0.85em; color: #555; }
       section, #skills, #contact { padding: 24px 0 !important;
                                     break-inside: avoid; }
       .hero-photo { filter: none !important; max-width: 200px; }
       .scroll-reveal { opacity: 1 !important; transform: none !important; }
     }

   Test: hit Ctrl+P in the browser — the printed/PDF version should look
   like a clean readable CV-style document with all URLs visible.
```

---

## Prompt 6 — Replace placeholder projects (run when you have content)

```
Working in c:\Users\USER\Desktop\PORTFOLIO\index.html. Replace the three
placeholder cards inside the <div class="projects-grid"> ... </div> in the
#projects section with real projects below.

For each project, render:
  - .project-thumb: an <img src="projects/<slug>.png" alt="<title>
    screenshot" loading="lazy" decoding="async" /> filling the thumb area
    (object-fit: cover, width: 100%, height: 100%). Drop the
    .project-thumb-pattern div and the .project-thumb-label span.
  - .project-title: project name
  - .project-desc: 1-2 sentence problem/outcome (what it does, who it helps)
  - .project-tags: one .project-tag span per tech
  - .project-links: GitHub link if provided; Live link if provided. Drop
    any link that's empty (don't ship href="#").

Also create the projects/ folder. The user will drop screenshots there.

Project content:

PROJECT 1
  Name: <FILL ME IN>
  Description: <1-2 sentences>
  Tech: <comma-separated>
  GitHub: <url or "none">
  Live: <url or "none">
  Slug (for screenshot filename): <kebab-case>

PROJECT 2
  Name: …
  Description: …
  Tech: …
  GitHub: …
  Live: …
  Slug: …

PROJECT 3
  Name: …
  Description: …
  Tech: …
  GitHub: …
  Live: …
  Slug: …

If the user gave fewer than 3 projects, render only the ones provided and
remove the extra cards (don't leave placeholders behind). If they gave more
than 3, keep all of them — the existing grid is responsive.
```

---

## Prompt 7 — Domain switch (run after buying a domain)

```
Working in c:\Users\USER\Desktop\PORTFOLIO. The user has registered a real
domain: <DOMAIN> (e.g., adamtrabelsi.com).

1. In index.html and 404.html, replace every occurrence of
   "https://adamtrabelsi.com" with "https://<DOMAIN>".

2. In sitemap.xml and robots.txt, replace the same.

3. In the JSON-LD <script type="application/ld+json"> block, update url
   and image fields.

4. In vercel.json, ensure no hardcoded host references conflict.

5. Verify the og:image URL resolves: from the project root, run
   `curl -I https://<DOMAIN>/og-image.png` (or .svg) and confirm 200.
   If it 404s, render og-image.svg → og-image.png and commit it.

6. Verify the social card by pasting the URL into:
   - https://www.opengraph.xyz/
   - https://cards-dev.twitter.com/validator
   Report back any warnings.

7. Verify the JSON-LD by pasting the URL into Google's Rich Results Test
   (https://search.google.com/test/rich-results) and report any errors.
```

---

## Notes for the User

- **Order matters.** Prompt 1 renames `portfolio.html` → `index.html`. After that, all subsequent prompts assume `index.html`.
- **Keep it static.** Resist the urge to introduce a build step, npm packages, or a framework. The whole site should stay one HTML file plus assets — that's its charm and its hosting flexibility.
- **Test after each prompt.** Open `index.html` in the browser, click every link, resize to phone width, run Lighthouse. Catching regressions one prompt at a time is cheaper than debugging at the end.
- **Don't run prompt 6 with placeholders.** Better to ship 3 dummy cards than 3 cards that say "Real Project (coming soon)" with `href="#"` — recruiters notice.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, single-page personal portfolio website for Minh Tuyen Nguyen, served via **GitHub Pages**. There is no build system, no package manager, no tests, and no server-side code in the repo — editing the HTML/CSS and pushing to `main` is the entire deploy pipeline.

## Deployment model

- GitHub Pages serves from the **`docs/` directory** on `main`. `docs/index.html` is the live site; the repo-root files are not served as the page.
- Custom domain is `build.nguyen365.com`, set by the `CNAME` files. **Both `CNAME` and `docs/CNAME` must stay in sync** — `docs/CNAME` is the one Pages actually reads since it serves from `docs/`.
- To preview locally, open `docs/index.html` in a browser, or run any static server from `docs/` (e.g. `python -m http.server` in that folder). No build step.
- `docs/index.html` sets `<meta name="robots" content="noindex, nofollow">` — the site is intentionally not indexed.

## Structure of `docs/index.html`

Everything lives in one ~3600-line file. It is the BootstrapMade **iPortfolio** template, lightly customized. Be aware of how it's organized before editing:

- **Lines ~1–2600 are inlined CSS**, the bulk of which is the auto-generated bootstrap-icons glyph map (`.bi-*::before { content: ... }`) and the template's stylesheet. Treat this region as vendored/generated; hand-edit it only when changing actual styling, and expect large unrelated blocks.
- **Content starts at `<body>` (~line 2606)**, organized as `<section>` blocks with stable `id`s: `header`, `hero`, `about`, `stats`, `skills`, `resume`, `portfolio`, `certifications`, `services`, `testimonials`, `contact`, `footer`. Navigation in `#navmenu` anchors to these ids — keep ids and nav links consistent when adding/removing sections.
- Most user-facing content edits happen inside these sections. Several sections still contain template placeholder text (lorem-ipsum, "A108 Adam Street", stock testimonial names) — when asked to update content, replace placeholders rather than assuming they're real.

## External dependencies (all CDN-hosted, nothing vendored locally)

- CSS/JS come from jsDelivr (Bootstrap 5.3.3, bootstrap-icons) and from **`bootstrapmade.com`** directly — including vendor libs (AOS, typed.js, purecounter, waypoints, glightbox, imagesloaded, isotope, swiper) and crucially `main.js` (loaded from a `bootstrapmade.com/content/demo/iPortfolio/...` URL). There is no local copy, so site behavior depends on those remote URLs staying up. If interactive behavior (animations, typing effect, portfolio filtering, carousel) needs changing, the logic is in that remote `main.js`, not in this repo.

## Known caveat

The contact form posts to `forms/contact.php` (`<form action="forms/contact.php">`), which **does not exist and cannot work on GitHub Pages** (static hosting, no PHP). If contact submission needs to function, it must be rewired to a third-party form service (Formspree, Getform, etc.) — don't assume the current form delivers mail.

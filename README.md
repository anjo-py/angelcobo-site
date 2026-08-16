# angelcobo.com

Personal narrative website for Angel Cobo, PhD. This is **not** a CV or resume
site — it's a small, editorial site that tells a first-person story, hosts
future writing, and gives people a way to get in touch.

Built with [Astro](https://astro.build), TypeScript, and plain CSS. Deploys as
a static site to Vercel.

## Project structure

```
src/
  content.config.ts       Content collection definitions (writing)
  content/
    writing/               MDX posts (essays, blurbs, reports)
  config/
    contact.ts             Formspree endpoint + social links — edit here
  layouts/
    BaseLayout.astro        Shared header/nav/footer
  pages/
    index.astro             Home / About — the narrative centerpiece
    writing/
      index.astro            Lists published (non-draft) posts, newest first
      [...slug].astro         Individual post template
    contact.astro           Contact form + links
  styles/
    global.css               Shared design tokens and base styles
public/
  favicon.svg, favicon.ico
```

## Running locally

```bash
npm install
npm run dev       # start local dev server
npm run build     # production build to dist/
npm run preview   # preview the production build locally
```

Requires Node.js 22+ (Astro's `create-astro` tooling and current Astro major
version require it). If you use `nvm`, run `nvm use 22` before the commands
above.

## Content: Writing

Writing lives in `src/content/writing/` as `.mdx` files. Frontmatter schema
(defined in `src/content.config.ts`):

```yaml
---
title: "Post title"
date: 2026-06-01
description: "One or two sentences for the index page and SEO."
draft: true       # true = not built, not linked, not reachable by URL
tags: ["tag-one", "tag-two"]
---
```

To add a new post, drop a new `.mdx` file in `src/content/writing/` and set
`draft: false` when it's ready to publish. Draft posts are excluded from
`getStaticPaths()` for the individual post route, so they don't get built or
served at all — not just hidden from the index list.

## Contact form

The contact form posts directly to [Formspree](https://formspree.io) via a
plain HTML `POST` (no client-side JS), which works fine with Astro's static
output.

Before launch:

1. Create a form at formspree.io.
2. Open `src/config/contact.ts` and replace `FORMSPREE_ENDPOINT` with your
   real endpoint URL.
3. Replace the placeholder URLs in `CONTACT_LINKS` (LinkedIn, Instagram, X,
   email) with your real links.

The form includes a hidden honeypot field (`_gotcha`) for basic spam
filtering — Formspree's standard convention. Leave it in place.

## Deployment

Static output, deployed to Vercel with a custom domain (angelcobo.com). No
Vercel adapter is needed — Vercel builds Astro's static output directly.
`astro.config.mjs` sets `site: 'https://angelcobo.com'` for correct canonical
URLs.

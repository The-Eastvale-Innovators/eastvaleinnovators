# Eastvale Innovators Website

This repository contains the public website for Eastvale Innovators, a student-led STEM organization focused on building real-world projects, organizing teams, and creating opportunities for collaboration and leadership. The site is a static, content-first experience designed to present the organization clearly to students, mentors, partners, and visitors.

## Overview

The website is organized as a collection of standalone HTML pages with shared assets, reusable header and footer partials, and small JavaScript helpers that keep navigation and layout behavior consistent. It presents the organization at three levels:

1. The brand and mission of Eastvale Innovators as a whole.
2. The teams and people who contribute to the organization.
3. The individual projects and initiatives the teams are building.

The site is intentionally broad in scope. It is not just a project showcase; it is the public home for the organization, its current work, and its identity.

## What Visitors Can Find

- A home page that introduces Eastvale Innovators and establishes the visual identity of the site.
- A projects page that groups initiatives by current and past work.
- Dedicated project pages for major efforts such as Roosevelt Connect, Intelligrader, and Virtual Medical Missions.
- A teams page that breaks the organization into its constituent groups.
- A leadership page for organizational leadership and structure.
- A contact page for outreach, partnerships, and general communication.

## Repository Layout

```
.
├── index.html                          # /          homepage and organization overview
├── contact.html                        # /contact   contact and outreach page
├── projects/
│   ├── index.html                      # /projects                       project gallery
│   ├── roosevelt-connect.html          # /projects/roosevelt-connect
│   ├── intelligrader.html              # /projects/intelligrader
│   ├── virtual-medical-missions.html   # /projects/virtual-medical-missions
│   └── phage-hunters.html              # /projects/phage-hunters
├── about/
│   ├── teams.html                      # /about/teams        team directory (with anchors)
│   ├── leadership.html                 # /about/leadership   leadership and roles
│   └── alumni.html                     # /about/alumni       alumni page
├── partials/                           # shared header + footer markup (fetched at runtime)
├── assets/                             # css, js, img, models (see "Assets And Content")
├── _templates/                         # scaffolds, e.g. project-template.html (not deployed)
├── vercel.json                         # Vercel hosting config (clean URLs, redirects, headers)
└── .vercelignore                       # files excluded from the Vercel deploy
```

Folder names become URL segments: `projects/index.html` serves at `/projects`
and `about/teams.html` serves at `/about/teams`. All internal links, asset
references, and partial fetches use root-absolute paths (`/assets/...`,
`/partials/...`, `/about/teams.html`), so pages work regardless of how deeply
they are nested.

Internal page links point at the real files (`/about/teams.html`, `/projects/`)
so navigation works on any static server or local preview, not just Vercel. In
production, `cleanUrls` transparently redirects those to the extensionless form
(`/about/teams`), so visitors still see clean URLs. When a page moves, add a
redirect in `vercel.json` so old links keep resolving (see the existing entries
there for the pattern).

## Shared Architecture

The site uses a lightweight shared-component pattern instead of a framework or build pipeline.

- `partials/header.html` contains the global navigation markup.
- `partials/footer.html` contains the shared footer content.
- `assets/js/shared-header.js` loads the header partial into each page.
- `assets/js/shared-footer.js` loads the footer partial into each page.
- `assets/js/site-common.js` handles shared UI behavior such as dropdown navigation, mobile menu interactions, navbar state, and theme toggling.
- `assets/css/shared-header.css` and `assets/css/shared-footer.css` provide the shared styling for those reused sections.

This structure keeps the pages visually aligned while still allowing each page to have its own layout, content, and styling.

## Navigation Model

The header is built to reflect the site’s content hierarchy rather than flatten everything into a single list.

- The main nav points to the homepage, projects, teams, and contact areas.
- The Projects dropdown separates past and current work.
- The Teams dropdown exposes anchors for each major team section.
- The mobile menu mirrors the desktop navigation and keeps the same grouping.

That structure makes the site easier to scan and keeps the most important destinations close together.

## Design Direction

The current site leans into a bold, high-contrast, futuristic STEM aesthetic. Across the pages, you will see:

- Deep navy and dark backgrounds with bright accent colors.
- Large display typography using Google Fonts such as Inter, Space Grotesk, Orbitron, and related families.
- Hero sections with gradients, overlays, and motion.
- Project pages that use strong color themes to distinguish each initiative.
- Card-based content sections and glass-like surfaces for hierarchy and readability.

The design language is meant to feel energetic and student-built, while still being polished enough to represent the organization publicly.

## Key Pages

### Home

The home page sets the tone for the whole site. It introduces the organization, gives visitors a fast sense of the brand, and points them to deeper content.

### Projects

The projects page collects the organization’s work into one place so visitors can understand what Eastvale Innovators is building without needing to hunt through multiple pages.

### Teams

The teams page is structured to help visitors understand how the organization is divided. Navigation links and page anchors point to:

- Eastvale Innovators Team
- Roosevelt Connect Team
- Intelligrader Team
- Virtual Medical Missions Team

### Leadership

The leadership page gives visibility to the student leaders guiding the organization and helps establish the organizational structure.

### Contact

The contact page exists for collaboration, questions, partnerships, and general outreach.

## Project Pages

The site currently highlights several initiatives with their own dedicated pages:

- Roosevelt Connect - a current project page focused on its own mission and presentation.
- Intelligrader - an AP History FRQ scoring tool with its own product-style landing page.
- Virtual Medical Missions - a past project page centered on outreach, operations, and support infrastructure.

The projects page and navigation also separate current and past work so visitors can understand what the organization is actively building versus what has already been completed.

## Assets And Content

- `assets/img/` contains the visual assets used across the site, including logos, portraits, and project imagery. `placeholder.jpg` is the fallback portrait used where a real image is not yet available.
- `assets/models/` contains 3D model assets (`gear.obj`) loaded on the homepage.
- `assets/css/` holds the shared styles: `pages.css`, `shared-header.css`, and `shared-footer.css`.
- `assets/js/` holds the shared behavior: the header/footer loaders, `site-common.js`, and `color-transition.js`.

When adding new content, keep assets organized by type and reuse existing visual patterns where possible. Remove assets that are no longer referenced rather than letting them accumulate.

## Content Conventions

- Use short, direct section headings.
- Prefer explicit page labels over generic wording.
- Keep project descriptions specific to the actual work being shown.
- Update navigation when a page becomes important enough to be discoverable site-wide.
- Keep the public-facing copy consistent with the tone used across the rest of the site.

## Local Development

This is a static site, but it must be served over HTTP — the header and footer are fetched at runtime with `fetch()` using root-absolute paths, which do not work from `file://`. Always serve from the repository root so `/partials/...` and `/assets/...` resolve correctly.

Either of these works:

```bash
# Plain static server (Python)
python -m http.server

# Vercel CLI — mirrors production, including clean URLs and headers
npx vercel dev
```

Then open the local address printed in the terminal.

If you are working on the shared header, footer, or JavaScript loaders, refresh pages after edits so the partials are reloaded from disk.

## How To Add A New Page

1. Copy `_templates/project-template.html` into `projects/` and rename it (e.g. `projects/my-project.html`, which serves at `/projects/my-project`).
2. Update the page metadata, title, and canonical URL.
3. Reuse the shared header and footer partials.
4. Add any page-specific assets under `assets/`.
5. Keep the styling aligned with the rest of the site unless the new page has a deliberate visual identity of its own.

## Editing Guidelines

- Keep the shared navigation and footer in sync with every page.
- Preserve the site-wide tone: student-driven, technical, and polished.
- Prefer clear page organization over adding more visual noise.
- Use the existing partials and shared scripts instead of duplicating markup.
- If you introduce a new project, update the navigation and project listings so the site stays coherent.
- Test changes on both desktop and mobile widths when modifying layout-heavy pages.

## Maintenance Checklist

When making a site-wide change, verify the following:

- The header and footer still load correctly on every page.
- The nav links still point to the right pages and anchors.
- Project pages still have correct meta tags and page titles.
- New assets are committed under the right folder in `assets/`.
- The homepage, projects page, and contact page still reflect the current state of the organization.

## Deployment

The site is hosted on **Vercel**. There is no build step — Vercel serves the repository’s HTML, CSS, JavaScript, and asset files directly. Every push to the production branch triggers an automatic deploy, and pull requests get their own preview URLs.

Configuration lives in `vercel.json`:

- `cleanUrls` serves pages without the `.html` extension (`/teams` instead of `/teams.html`) and redirects the old extension to the clean path.
- `trailingSlash` is disabled for consistent, canonical URLs.
- Security headers (`X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`) are applied to every response.

`.vercelignore` keeps scaffolding (`_templates/`) and this README out of the deployed output.

### Custom domain

The custom domain (`www.eastvaleinnovators.club`) is configured in the **Vercel dashboard** under the project’s Domains settings, not via a `CNAME` file. Point the domain’s DNS at Vercel and add it in the dashboard; Vercel provisions the TLS certificate automatically.

## Why This Site Exists

The purpose of the website is to give Eastvale Innovators a single public home that communicates:

- Who the organization is.
- What the organization is building.
- Who is involved.
- How people can get in touch or collaborate.

In other words, the site should function as the organization’s front door, archive, and project showcase all in one.

## Credits

The footer credits the site’s creators and supporting contributors. Keep those names current if the ownership or maintenance structure changes.
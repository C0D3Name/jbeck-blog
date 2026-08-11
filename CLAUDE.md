# jbeck-blog

Hugo-based professional blog for **jbeck.me** — the authority site and writing
platform for Jonathan Beck, software engineer since 2007.

Inherits the shared development context at `C:\dev\CLAUDE.md`.

## Purpose

- **jbeck.me** is the person: writing, authority, portfolio
- **bitflection.com** is the company: contracting vehicle, service offerings
- Keep the two separate and cross-linked. GovCon artifacts (SAM.gov, UEI, CAGE,
  NAICS) belong on the company site, not here
- Content spans .NET/C#, Azure, AI architecture and benchmarking, AI-assisted
  development pipelines, and game development as a hobby
- The site supports contract and W2 acquisition, targeting Healthcare, VA, and
  government contracting

## Stack

| Layer | Choice |
|---|---|
| Static site generator | Hugo **Extended** (Extended is required — Blowfish uses SCSS) |
| Theme | Blowfish, pinned as a git submodule at `themes/blowfish` |
| Hosting | Azure Static Web Apps, Free tier |
| DNS | Azure DNS; domain registered at Squarespace |
| CI/CD | GitHub Actions |
| Tooling | .NET console apps under `tools/` |

Rationale for every choice is recorded in `docs/decisions.md`. Read it before
proposing a stack change — several obvious-looking alternatives were evaluated
and rejected for specific reasons.

## Commands

```
hugo server --buildDrafts     Local dev at localhost:1313, live reload
hugo server                   Exactly what production will publish
hugo --gc --minify            Production build into public/
hugo new content blog/<slug>/index.md    New post as a page bundle
```

Clone requires `--recurse-submodules`, or run
`git submodule update --init --recursive` after the fact.

## Structure

```
config/_default/     Site configuration (hugo, params, languages, menus, markup)
content/
  _index.md            Homepage
  blog/                Posts, one page bundle per post
  about/
  work-with-me/
  experience/
assets/img/          Images processed by Hugo
archetypes/blog.md   Front matter template for new posts
themes/blowfish/     Theme submodule
tools/               .NET console tooling
docs/                Tracked project docs
docs/local/          Local-only working notes, never committed
```

## Conventions

### Never edit the theme

`themes/blowfish/` is a submodule. Override instead:

- **Config** — `config/_default/params.toml`
- **Styles** — `assets/css/custom.css`
- **Layouts** — mirror the theme's path under `layouts/`

### URLs

Posts are `/blog/:slug/` with **no date segment**, so evergreen technical
content does not look stale. Dates still render on the post itself as a trust
signal; set `lastmod` when revising.

### Content

Posts are page bundles (`content/blog/<slug>/index.md`) so images live beside
the post. New posts default to `draft: true`.

### Accessibility

**WCAG 2.1 Level AA is a build requirement, not an aspiration.** Section 508
conformance is a commercial requirement for the VA and GovCon work this site
supports, and a site that fails its own accessibility gate cannot make that
claim.

- Meaningful alt text on every image
- Correct heading order, never skipped for styling
- Verify contrast ratios before changing any color

### Local-only content

`docs/local/` is gitignored and **must never be committed**. It holds the site
spec, source images, working status notes, and the DNS runbook containing live
mail configuration.

## Constraints

- The `jbeck.me` zone carries **live Google Workspace mail**. Any DNS change can
  break it. Follow `docs/local/dns-migration-runbook.md` and never reorder its
  cutover steps.
- **Do not publish a PDF resume**, home address, or phone number. Published
  resumes are scraped for identity theft. Full resume on request only.
- Verify the site on its `*.azurestaticapps.net` URL before any DNS cutover.

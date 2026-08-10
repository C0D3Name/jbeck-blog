# jbeck-blog

Source for [jbeck.me](https://jbeck.me) — the professional blog of Jonathan Beck.

Built with Hugo and the Blowfish theme. Deployed to Azure Static Web Apps.

## Stack

| Layer | Choice |
|---|---|
| Static site generator | Hugo Extended 0.164.0 |
| Theme | [Blowfish](https://github.com/nunocoracao/blowfish) v2.105.0 (git submodule) |
| Hosting | Azure Static Web Apps (Free tier) |
| DNS | Azure DNS (domain registered at Squarespace) |
| CI/CD | GitHub Actions |
| Tooling | .NET 10 console apps |

See [docs/decisions.md](docs/decisions.md) for why.

## Prerequisites

- **Hugo Extended** — `winget install Hugo.Hugo.Extended`
  The Extended build is required; Blowfish uses SCSS.
- **.NET 10 SDK** — for the tooling in `tools/`
- **Git** — the theme is a submodule

## Getting started

Clone with submodules:

```
git clone --recurse-submodules <repo-url>
```

If already cloned without them:

```
git submodule update --init --recursive
```

Run locally:

```
hugo server --buildDrafts
```

Serves at http://localhost:1313 with live reload. Drop `--buildDrafts` to see
exactly what production will publish.

Build:

```
hugo --gc --minify
```

Output goes to `public/` (gitignored).

## Structure

```
config/_default/     Site configuration
  hugo.toml            Core settings, permalinks, taxonomies
  params.toml          Theme options
  languages.en.toml    Site title, author, social links
  menus.en.toml        Navigation
content/
  _index.md            Homepage
  blog/                Posts (page bundles)
  about/
  work-with-me/
  experience/          Draft — not yet published
assets/img/          Images processed by Hugo
archetypes/blog.md   Front matter template for new posts
themes/blowfish/     Theme (submodule — do not edit)
docs/                Tracked project docs
docs/local/          Local-only working notes (gitignored)
```

## Writing

Create a post:

```
hugo new content blog/my-post-slug/index.md
```

This creates a page bundle, so images live beside the post. New posts are
`draft: true` — remove it to publish.

URLs are `/blog/my-post-slug/` with no date segment, so evergreen posts do not
look stale. Dates still render on the post itself as a trust signal; set
`lastmod` when you revise one.

Front matter fields that matter:

```yaml
date: 2026-08-10        # publication date, drives sort order
lastmod: 2026-09-01     # renders as "Updated"
draft: true             # excluded from production builds
tags: []
categories: []
series: []              # for multi-part posts
```

## Customizing the theme

Never edit `themes/blowfish/`. Override instead:

- **Config** — `config/_default/params.toml`
- **Styles** — `assets/css/custom.css`
- **Layouts** — mirror the theme path under `layouts/`

## Accessibility

This site targets **WCAG 2.1 Level AA**. That is a deliberate commitment, not
incidental — accessibility conformance is a requirement for the federal and VA
work this site supports.

Keep it that way: meaningful alt text on every image, correct heading order,
sufficient contrast on any color change.

## Deployment

Deploys to Azure Static Web Apps via GitHub Actions on push. Not yet wired up —
see [docs/decisions.md](docs/decisions.md).

DNS cutover steps live in `docs/local/dns-migration-runbook.md` (local only —
it contains mail configuration).

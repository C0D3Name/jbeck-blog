# Decisions

Why this site is built the way it is. Newest first.

## Hugo over Statiq (2026-08-10)

**Decision:** Build on Hugo rather than Statiq, the .NET static site generator.

**Why:** Statiq was the on-brand choice for a C#/.NET engineer, and its
document-pipeline model genuinely is more flexible than Hugo's. The maintenance
signals ruled it out:

- Last release **2024-01-09** — still `1.0.0-beta` after roughly six years
- Latest published packages: `Statiq.Web 1.0.0-beta.60`, `Statiq.Core 1.0.0-beta.72`
- .NET 10 support exists only in `main`, never released — using it means
  building from source and maintaining private packages
- Bus factor of one: 2,648 commits from the maintainer, 93 from the next contributor
- `Statiq.Docs` (the API-doc generator) dormant since January 2024

The project is not abandoned — issues were still being triaged in mid-2026 —
but nothing has shipped in over two years. Not a foundation for a site meant to
be low-maintenance for years.

**Consequence:** The .NET story is carried by the *pipeline* instead of the
generator — Azure hosting, GitHub Actions, and .NET console tooling in `tools/`.
That delivers the credibility without the dependency risk.

**Revisit if:** Statiq ships a 1.0 with a real release cadence and more than one
active maintainer.

## Blowfish theme (2026-08-10)

**Decision:** Blowfish v2.105.0, pinned as a git submodule.

**Why:** Held to the same maintenance standard applied to Statiq. Blowfish
releases roughly monthly (v2.103 May, v2.104 July 2, v2.105 July 26, 2026) with
22 open issues. PaperMod is more popular (13.8k stars) but its last release was
November 2024. Blowfish is the one actively shipping.

Also supports article series, related content, and a built-in appearance
switcher, all useful for multi-part technical writing.

## Undated post URLs (2026-08-10)

**Decision:** `/blog/:slug/` rather than `/blog/:year/:month/:slug/`.

**Why:** Dated URLs visibly age evergreen technical content and measurably
depress click-through on posts that are still correct years later.

**Nuance:** Dates still render *on the post*. For AI and tooling topics a date
is a trust signal — readers are right to distrust undated writing about
fast-moving subjects. `lastmod` marks revised posts as maintained.

## Azure Static Web Apps for hosting (2026-08-10)

**Decision:** Azure Static Web Apps, Free tier, deployed by GitHub Actions.

**Why:** Free, custom domains with managed TLS, and PR preview environments.
Aligns with the Azure/.NET positioning this site supports — "runs on Azure with
a GitHub Actions pipeline" is a demonstrable claim, and a post in itself.

## Azure DNS for the zone (2026-08-10)

**Decision:** Move nameservers to Azure DNS. Registration stays at Squarespace.

**Why:** Apex domains cannot use a CNAME. Squarespace DNS supports no
ALIAS/ANAME record, so `jbeck.me` could not point at Static Web Apps from
there. Azure DNS provides ALIAS records and keeps DNS, hosting, and TLS in one
portal. Cost is roughly $6/year.

**Risk:** The zone carries live Google Workspace mail. MX, SPF, and DKIM must be
recreated before the nameserver change or mail breaks. Steps and captured record
values are in `docs/local/dns-migration-runbook.md`.

## WCAG 2.1 AA as a standing requirement (2026-08-10)

**Decision:** Treat accessibility conformance as a build requirement, not a
nice-to-have. Enforce it in CI.

**Why:** Section 508 requires federal ICT to meet WCAG Level AA, and the VA
enforces it strictly. For a contractor targeting VA and GovCon work,
demonstrated accessibility competence is a source-selection discriminator that
most .NET contractors cannot speak to credibly.

A site that fails its own CI accessibility gate is a claim that cannot be made.

**Scope note:** A static blog passing AA is not hard. The credibility comes from
writing about the difficult parts — forms, ARIA live regions, dynamic content,
and PDF remediation — not from a Lighthouse score.

## No published PDF resume (2026-08-10)

**Decision:** Publish an HTML Experience page with no personal identifiers. No
downloadable resume. Full resume on request.

**Why:** Published resumes are routinely scraped and reused in fraudulent job
postings and identity-theft kits. Withholding home address and phone number
costs nothing in credibility, and "available on request" starts a conversation
rather than a silent download.

## Site and company split (2026-08-10)

**Decision:** jbeck.me is the person — writing, authority, and portfolio.
bitflection.com is the company — contracting vehicle and service offerings.

**Why:** GovCon buyers need SAM.gov registration, UEI, CAGE, and NAICS codes.
That belongs on the company site. Mixing the two weakens both. Cross-link them.

# Blog Backup (`blog.benweston.site`) - Agent Instructions & Project Context

## Project Overview
This repository (`benweston/blog-backup`) is the automated inbound backup target for Ben Weston's technical writing hub at [blog.benweston.site](https://blog.benweston.site/), paired with the outbound authoring repository (`benweston/blog-source`) and the main engineering portfolio at [benweston.site](https://benweston.site/) (`benweston/app-portfolio`). Articles are authored in `blog-source`, saved as drafts or published in Hashnode, and automatically synchronised back to the root of this repository as `<cuid>.md` files via Hashnode's native GitHub backup integration.

---

## Core Engineering Pillars & Editorial Focus

Articles and technical series align with the three-tier engineering workflow defined across Ben's portfolio:
1. **Linux & Systems:** Enterprise Linux operations across Red Hat, SUSE, and Debian families (and their derivatives), covering system administration, automation scripting, security hardening, and performance monitoring.
2. **Software & DX:** Workstation/developer tooling (`dx-*`), automated CI/CD pipelines, robust Bash & shell scripting for workflow automation, and Python for systems and software development.
3. **Cloud & Architecture:** Designing resilient, cost-conscious solutions across Google Cloud, Infrastructure as Code with Terraform, containerised services (Docker), and edge delivery.

> [!NOTE]
> **Stack Scope:** TypeScript is utilised strictly as internal tooling in the companion Astro portfolio (`app-portfolio`). It is **not** part of Ben's highlighted personal engineering stack. Do not feature TypeScript as a core personal skill in editorial bios, profile copy, or pillar summaries.

---

## Architecture & Publishing Pipeline

The technical writing hub is intentionally decoupled from `app-portfolio` on its own dedicated subdomain (`blog.benweston.site`), backed by a two-way GitHub pipeline:

- **Outbound Source (`benweston/blog-source`):** Markdown articles with YAML frontmatter are authored in `posts/` and published or updated automatically on Hashnode via `.github/workflows/publish-to-hashnode.yml` (`Hashnode/publish-github-action@v1`).
- **Inbound Backup (`benweston/blog-backup`):** Hashnode's native GitHub integration synchronises published posts back to the root of this dedicated backup repository (`<cuid>.md`).
- **Portfolio Integration (`benweston.site/blog`):** Visitors navigating to `benweston.site/blog` (or clicking **Blog ↗** in the portfolio header) are routed via HTTP `301` edge redirects in `app-portfolio` to `https://blog.benweston.site`.
- **Custom Domain & Edge Routing:**
  - `CNAME` record `blog` → `hashnode.network` managed in Squarespace DNS.
  - `TXT` verification record `_vercel` verifies domain ownership on Hashnode's edge router.
- **Future Transition Path (`app-blog`):** Because all articles are authored and backed up as standard Markdown with YAML frontmatter in Git, migrating to a self-hosted Astro application (`app-blog` on Firebase Hosting) requires zero content rewriting — only repointing the `blog` DNS record.

---

## Directory Structure

```text
blog-backup/
├── assets/                 # Repository-level assets (banners, diagrams, screenshots)
│   ├── banners/            # Repo header (main-banner.png)
│   ├── diagrams/           # Architecture diagrams and schematics
│   ├── icons/              # Logos, badges, and iconography
│   ├── misc/               # Miscellaneous asset files
│   └── screenshots/        # Project, CLI, and UI screenshots
├── docs/                   # Project documentation & backup guides
│   ├── backup-and-recovery.md # Hashnode backup mechanics and file format specification
│   └── README.md           # Documentation index
├── <cuid>.md               # Automated Hashnode post backups at repository root
├── .gitignore              # Git ignore rules for POSIX/Linux, Sublime Text, and secrets
├── GEMINI.md               # Persistent agent guidelines and project context
├── LICENSE                 # MIT Licence
└── README.md               # Repository overview and documentation links
```

---

## Backed-Up Post Specification

Every backed-up article is written by Hashnode to the repository root as `<cuid>.md` with YAML frontmatter:

```yaml
---
title: "Engineering benweston.site: From Zero-Runtime Foundations (v1.0.0) to a Multi-Page Platform (v2.0.0)"
seoTitle: "Engineering benweston.site: From v1.0.0 to v2.0.0"
seoDescription: "A deep dive into the engineering journey behind benweston.site..."
datePublished: 2026-10-22T16:02:11.984Z
cuid: cmvjqbmap000002kv5csd638r
slug: engineering-benweston-site-from-v1-to-v2
cover: https://cdn.hashnode.com/uploads/covers/...
tags: web-development, google-cloud, cicd, github-actions, platform-engineering
---
```

### Key Backup Mechanics
- **Root-Level `<cuid>.md` Files:** Hashnode's GitHub backup integration writes and updates files at the root of the repository using the immutable post `cuid` as the filename. Do **not** delete, rename, or move `<cuid>.md` files into subdirectories, as Hashnode expects them at the root when pushing updates.
- **Read-Only Backup Target:** Content edits should always be performed in `benweston/blog-source` (`posts/<slug>.md`) and pushed to Hashnode, which will then automatically update the corresponding `<cuid>.md` file here.

---

## Coding, Editorial & Agent Guidelines

- **Git, Publishing & Ticket Closure Consent (CRITICAL):** Agents **must never** execute `git commit`, `git push`, `git tag`, or close GitHub issues (including using `closes #<n>` in commit messages) without explicit prior instruction and consent from the user for that specific action. All work must remain in the local working tree unless the user explicitly asks to commit or push.
- **Language & Locale:** Standardise on British English (`en-GB`) for all articles, copy, documentation, and frontmatter descriptions (e.g. *optimised*, *containerised*, *virtualisation*, *colour*, *standardised*).
- **Google Cloud Naming:** Use "Google Cloud", never "Google Cloud Platform" or "GCP".
- **Accessible & Semantic Markdown:** Use logical heading hierarchies (`##`, `###`, never skipping levels), descriptive link text, fenced code blocks with explicit language identifiers, and meaningful `alt` attributes for all images.
- **Asset & Backup Boundaries:** Do **not** overwrite, delete, or relocate binary files in `assets/` (e.g. `main-banner.png`) or backed-up `<cuid>.md` files at the repository root without explicit user permission.
- **Privacy & Security:** Never include personal email addresses, internal hostnames/IPs, or sensitive credentials/tokens in posts, documentation, wikis, or source files. Always use generic role titles (e.g. *Project Owner*, *Repository Administrator*) and RFC 5737 / RFC 2606 example domains/addresses where needed.
- **Documentation Preservation:** Maintain documentation integrity. Keep existing comments, frontmatter metadata, licenses, and references intact unless instructed otherwise.

---

## Standardised Issue & Ticket Specification

All issues, user stories, and project board cards MUST strictly follow the exact 4-section structure below. No deviations or ad-hoc formats are permitted. Acceptance criteria represent hard requirements that must be verified prior to promoting any work to Done.

```markdown
### Overview

[Concise 1–2 sentence statement of purpose, context, and outcome]

### Requirements & Scope

1. **[Area 1]:** [Specific deliverable]
2. **[Area 2]:** [Specific deliverable]
3. **[Area 3]:**
   - **[Sub-item A]:** [Details]
   - **[Sub-item B]:** [Details]
4. **[Area 4]:** [Specific deliverable]

### Technical Considerations

- [Editorial, structural, frontmatter, or asset constraints]
- [Publishing workflow or Hashnode CDN considerations]
- [Standards compliance (e.g. en-GB locale, accessibility, Google Cloud naming)]

---

### Acceptance Criteria

- [ ] Markdown frontmatter and formatting validated cleanly.
- [ ] [Verifiable check 2]
- [ ] [Verifiable check 3]
- [ ] Published and verified on `blog.benweston.site`.
```

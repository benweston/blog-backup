<a href="https://github.com/benweston/blog-backup">
    <img
        src="/assets/banners/main-banner.png"
        alt="Ben Weston - Cloud & Software Engineering"
        width="1100"
        height="150"
        loading="lazy"
    />
</a>

# Backup & Synchronisation from Hashnode

<div align="justify">

This repository (`benweston/blog-backup`) serves as the automated inbound backup target for technical articles published to [blog.benweston.site](https://blog.benweston.site/). Articles are authored in [`benweston/blog-source`](https://github.com/benweston/blog-source), synced to Hashnode via GitHub Actions, and automatically backed up here via Hashnode's native GitHub integration whenever a post is published or updated.

</div>

## Two-Way Pipeline Architecture

| Stage | Repository / Platform | Direction | Mechanism |
| :--- | :--- | :--- | :--- |
| **1. Authoring** | [`benweston/blog-source`](https://github.com/benweston/blog-source) | Outbound (`posts/*.md`) | `Hashnode/publish-github-action@v1` on push to `main` |
| **2. Publication** | [blog.benweston.site](https://blog.benweston.site/) (Hashnode) | Live / Drafts | Cover selection (Unsplash/CDN), draft review, and publication |
| **3. Backup** | [`benweston/blog-backup`](https://github.com/benweston/blog-backup) | Inbound (`/<cuid>.md`) | Native Hashnode GitHub Backup integration |

## Backed-Up File Format

When a post is published or updated on Hashnode, Hashnode commits a Markdown file directly to the root of the `main` branch named after the post's unique Hashnode `cuid` (e.g. `cmvjqbmap000002kv5csd638r.md`).

Each backed-up file includes Hashnode-generated YAML frontmatter followed by the published Markdown body:

```yaml
---
title: "Article Title"
seoTitle: "SEO Title"
seoDescription: "SEO Description"
datePublished: 2026-10-22T16:02:11.984Z
cuid: cmvjqbmap000002kv5csd638r
slug: article-slug
cover: https://cdn.hashnode.com/uploads/covers/...
tags: tag-1, tag-2, tag-3
---
```

| Field | Description |
| :--- | :--- |
| `title` | Published title of the article |
| `seoTitle` | Custom SEO title configured in source frontmatter or Hashnode |
| `seoDescription` | Meta description for search engines and Open Graph |
| `datePublished` | ISO 8601 timestamp when the article was first published |
| `cuid` | Immutable Hashnode post identifier (matches the `<cuid>.md` filename) |
| `slug` | URL slug linking the post to `blog-source` and `blog.benweston.site/<slug>` |
| `cover` | Resolved Hashnode CDN URL for the article cover image |
| `tags` | Comma-separated list of published Hashnode tag slugs |

## Operational Guidelines

- **Do Not Rename or Move `<cuid>.md` Files:** Hashnode tracks backed-up articles at the repository root by `<cuid>.md`. Moving or renaming these files will cause Hashnode to create duplicate files on subsequent updates.
- **Canonical Editing Location:** Always author and edit articles in [`benweston/blog-source`](https://github.com/benweston/blog-source) rather than editing `<cuid>.md` files directly in `blog-backup`.

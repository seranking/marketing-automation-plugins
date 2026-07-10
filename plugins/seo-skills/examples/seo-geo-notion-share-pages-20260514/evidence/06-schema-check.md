# 06 — Schema check (evidence)

> Source: `mcp__firecrawl-mcp__firecrawl_scrape(url="https://www.notion.com/help/public-pages-and-web-publishing", formats=["rawHtml"])` on 2026-05-14. **This evidence is real, not gated.**

## Detected blocks

Two `<script type="application/ld+json">` blocks present.

### Block 0 — BreadcrumbList

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Help, Support, and Documentation for Notion",
      "item": "https://www.notion.com/help"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Publish a Notion Site – Notion Help Center",
      "item": "https://www.notion.com/help/public-pages-and-web-publishing"
    }
  ]
}
```

Status: valid. Two-level breadcrumb is appropriate for a help-center article.

### Block 1 — Article

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Publish a Notion Site – Notion Help Center",
  "image": "https://img.youtube.com/vi/mvaNaZ4-E4c/maxresdefault.jpg"
}
```

Status: **minimal — present but missing every recommended field.**

### Validation gaps (Article)

| Property | Required? | Present? | Notes |
|---|---|---|---|
| `@context` | required | ✓ | |
| `@type` | required | ✓ Article | |
| `headline` | required | ✓ | |
| `image` | required | ✓ (YouTube thumbnail) | Image should be a content-relevant image, ideally ≥1200×800. YouTube `maxresdefault.jpg` is 1280×720 — borderline; a custom screenshot would be stronger. |
| `author` | recommended | ✗ | **Highest-priority fix.** Add Person or Organization. |
| `datePublished` | recommended | ✗ | **Highest-priority fix.** |
| `dateModified` | recommended | ✗ | **Highest-priority fix.** |
| `publisher` | recommended | ✗ | Should reference Notion as Organization. |
| `mainEntityOfPage` | recommended | ✗ | Self-canonical pointer. |
| `articleBody` | recommended | ✗ | Optional but helps disambiguation. |
| `articleSection` | recommended | ✗ | "Help" or "Sharing & publishing". |

### Recommended Article patch (paste-ready)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Publish a Notion Site",
  "image": {
    "@type": "ImageObject",
    "url": "{REPLACE: 1200x800 screenshot URL of Notion Sites publish UI}",
    "width": 1200,
    "height": 800
  },
  "author": {
    "@type": "Organization",
    "name": "Notion",
    "url": "https://www.notion.com"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Notion",
    "url": "https://www.notion.com",
    "logo": {
      "@type": "ImageObject",
      "url": "https://www.notion.com/images/notion-logo.png"
    }
  },
  "datePublished": "{REPLACE: ISO 8601 datePublished from CMS}",
  "dateModified": "{REPLACE: ISO 8601 last-edit date from CMS}",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://www.notion.com/help/public-pages-and-web-publishing"
  },
  "articleSection": "Sharing & publishing"
}
</script>
```

The three `{REPLACE: ...}` placeholders must be filled by the CMS team — the image URL and the two dates are page-specific.

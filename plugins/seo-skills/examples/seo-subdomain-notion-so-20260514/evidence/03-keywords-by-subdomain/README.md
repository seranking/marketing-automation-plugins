# Per-subdomain top keyword files

For each subdomain in scope, a production run produces one Markdown file with the top 100 ranking organic keywords:

- `www-notion-com.md` — apex (marketing, help, blog, customers, templates, releases)
- `www-notion-so.md` — legacy apex (declining; mostly redirects + user-workspace URLs)
- `developers-notion-com.md` — API/SDK queries
- (sitemaps.notion.com and app.notion.com don't get keyword files — operational, ≈ 0 ranking keywords)

Each file would emit a table:

```markdown
| Keyword | Position | Volume | Traffic est. | URL | Intent | Cluster |
|---|---|---|---|---|---|---|
| notion templates | 1 | 110,000 | 38,500 | https://www.notion.com/templates | T | templates |
| notion ai | 2 | 60,500 | 12,100 | https://www.notion.com/product/ai | C | product/ai |
| ... |
```

Source: `DATA_getDomainKeywords(domain="<subdomain>", limit=100, order_field="traffic")`.

**Pending SE Ranking auth.** The structure is in place; the metric rows would populate on re-run.

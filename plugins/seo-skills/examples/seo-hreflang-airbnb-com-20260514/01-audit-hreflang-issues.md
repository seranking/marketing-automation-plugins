# 01 — SE Ranking audit hreflang findings (evidence)

> Source: would come from `DATA_listAudits` + `DATA_getAuditReport` + `DATA_getAuditPagesByIssue` (filter: hreflang issue codes). SE Ranking auth-gated in this worktree.

## What a full run captures

For each hreflang-flagged issue code in SE Ranking's audit:

| Issue code | Description | Affected pages | Severity |
|---|---|---|---|
| `hreflang_missing_x_default` | No `x-default` alternate set | (pending) | High |
| `hreflang_no_return_tag` | Listed alternate doesn't reciprocate | (pending) | High |
| `hreflang_invalid_lang_code` | ISO 639-1 / 3166-1 violation | (pending) | Medium |
| `hreflang_conflict` | Same hreflang value on multiple distinct URLs | (pending) | High |
| `hreflang_no_self_reference` | Page doesn't list itself in its own hreflang set | (pending) | High |
| `hreflang_canonical_mismatch` | Canonical points elsewhere on a page with hreflang | (pending) | Critical (entire set ignored) |

The HTML-level evidence in `02-per-url-hreflang.md` already confirms `hreflang_missing_x_default` fires for every sampled page. A live SE Ranking run would expose how the *whole-site* audit ranks the gravity (e.g. "this issue affects 38,000 pages") and whether other codes fire too.

## Why this evidence file is intentionally light

`02-per-url-hreflang.md` is the load-bearing one for the example. SE Ranking's audit is a complementary feed that scales the HTML-level finding to the whole crawl.

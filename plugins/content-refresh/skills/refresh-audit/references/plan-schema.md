# refresh-plan.json schema

This is the exact structure to write in Phase 4. It's the contract with `refresh-update` — field names must match exactly, since `refresh-update` reads this file verbatim with no re-interpretation.

```json
{
  "version": 1, "created": "YYYY-MM-DD", "run_folder": ".../", "status": "pending",
  "user_context": "... or null", "voice_reference": "brand-voice.md path or null",
  "pages": [{
    "url": "...", "slug": "...", "meta_title": "... (the page's <title>/meta title — NOT the H1; keep these two distinct so refresh-update never mistakes one for the other)", "target_keyword": "...",
    "diagnosis": {"primary_cause": "...", "secondary": null, "confidence": "high", "evidence": ["..."]},
    "recommended_depth": "section_refresh",
    "outcome": "update", "merge_target": null,
    "preservation_locks": {"h1_keyword": "...", "snippet_passages": [], "internal_links": [], "schema_types": []},
    "change_map": [{"section": "H2: ...", "op": "modify", "what": "...", "why": "...", "research_refs": ["research-{slug}.md#F3"]}],
    "metadata_proposal": {"title": "...", "description": "..."},
    "gsc_baseline": {"checked": "YYYY-MM-DD", "days": 90, "clicks": 0, "impressions": 0, "ctr": 0.0, "avg_position": 0.0},
    "verdict": "pending", "approved_depth": null, "reviewer_notes": []
  }]
}
```

Field notes:

- `meta_title` is the page's `<title>`/meta title, deliberately never called just `title` — that ambiguity once caused `refresh-update` to use it as an H1. Never let this field's value become an H1 anywhere downstream.
- `gsc_baseline` is `null` whenever GSC wasn't connected/bound for this run — never fabricate it. When present, it's the number to compare against on the 6–8-week recheck (`refresh-update`'s Phase 4 carries it forward for exactly this reason).
- `verdict`, `approved_depth`, and `reviewer_notes` start empty/null and are filled in only at the Phase 5 approval gate — never populate them during Phase 1–4.
- `change_map` operations must reference research entries by ID (`research-{slug}.md#F{n}`) for every `modify`/`add` — never leave a change unsourced.

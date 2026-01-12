```yaml
number: 17498
title: "[`refurb`] Add fix safety section (`FURB129`) "
type: pull_request
state: closed
author: Kalmaegi
labels:
  - documentation
assignees: []
base: main
head: doc_fix_safety_for_readlings_in_for
created_at: 2025-04-20T15:15:26Z
updated_at: 2025-04-26T15:23:38Z
url: https://github.com/astral-sh/ruff/pull/17498
synced_at: 2026-01-12T15:56:02Z
```

# [`refurb`] Add fix safety section (`FURB129`) 

---

_@Kalmaegi_


## Summary

This PR add the fix safety section for rule `FURB129` in `readlines_in_for.rs` for #15584

---

_Comment by @github-actions[bot] on 2025-04-20 15:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @dhruvmanila on 2025-04-21 15:10_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:33_

---

_Comment by @dylwil3 on 2025-04-26 15:23_

Thanks for looking into this! I don't think I understand the reasoning you provided - both `readlines` and directly iterating will consume the stream.

In fact I think this fix should actually be marked as _safe_ , so I'm closing in favor of https://github.com/astral-sh/ruff/pull/17644 (see there for a justification of why I think it ought to be safe).

---

_Closed by @dylwil3 on 2025-04-26 15:23_

---

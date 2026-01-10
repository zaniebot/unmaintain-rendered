```yaml
number: 19432
title: "[`pep8-naming`] Fix `N802` false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-19422
created_at: 2025-07-19T15:36:06Z
updated_at: 2025-07-23T16:04:44Z
url: https://github.com/astral-sh/ruff/pull/19432
synced_at: 2026-01-10T17:58:13Z
```

# [`pep8-naming`] Fix `N802` false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`

---

_Pull request opened by @danparizher on 2025-07-19 15:36_

## Summary

Fixes #19422


---

_Comment by @github-actions[bot] on 2025-07-19 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-07-23 06:50_

---

_Label `rule` added by @ntBre on 2025-07-23 14:47_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:112 on 2025-07-23 14:51_

I think we can still use `matches!` here:

```suggestion
                    matches!(
                        name.segments(),
                        ["http", "server", "BaseHTTPRequestHandler" | "CGIHTTPRequestHandler" | "SimpleHTTPRequestHandler"]
                        )
```

You probably don't want to accept this through GitHub since it's formatted so poorly 😆 

---

_@ntBre approved on 2025-07-23 14:52_

Looks good, thank you! Just one minor simplification suggestion.

---

_Renamed from "[`pep8_naming`] Fix N802 false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`" to "[`pep8-naming`] Fix `N802` false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`" by @ntBre on 2025-07-23 15:49_

---

_Review requested from @ntBre by @danparizher on 2025-07-23 16:03_

---

_@ntBre approved on 2025-07-23 16:04_

---

_Merged by @ntBre on 2025-07-23 16:04_

---

_Closed by @ntBre on 2025-07-23 16:04_

---

_Branch deleted on 2025-07-23 16:04_

---

```yaml
number: 15372
title: "[red-knot] Minor refactor of red_knot_vendored/build.rs"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/zip-typeshed-dir
created_at: 2025-01-09T12:19:07Z
updated_at: 2025-01-09T12:25:20Z
url: https://github.com/astral-sh/ruff/pull/15372
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Minor refactor of red_knot_vendored/build.rs

---

_@sharkdp_

## Summary

See https://github.com/astral-sh/ruff/pull/15370#discussion_r1908611461:

- Rename `zip_dir` to `write_zipped_typeshed_to` to clarify it's not a generic function (anymore)
- Hard-code `TYPESHED_SOURCE_DIR` instead of using a `directory_path` argument


---

_Label `red-knot` added by @sharkdp on 2025-01-09 12:19_

---

_Review requested from @carljm by @sharkdp on 2025-01-09 12:19_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-09 12:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-09 12:19_

---

_Label `internal` added by @sharkdp on 2025-01-09 12:19_

---

_@AlexWaygood approved on 2025-01-09 12:20_

Perfect, thanks!

---

_Merged by @sharkdp on 2025-01-09 12:23_

---

_Closed by @sharkdp on 2025-01-09 12:23_

---

_Branch deleted on 2025-01-09 12:23_

---

_Comment by @github-actions[bot] on 2025-01-09 12:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

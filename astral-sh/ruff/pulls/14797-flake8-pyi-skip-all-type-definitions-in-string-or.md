```yaml
number: 14797
title: "[`flake8-pyi`] Skip all type definitions in `string-or-bytes-too-long (PYI053)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi053-type-defs
created_at: 2024-12-05T21:23:21Z
updated_at: 2024-12-06T00:48:55Z
url: https://github.com/astral-sh/ruff/pull/14797
synced_at: 2026-01-12T15:55:49Z
```

# [`flake8-pyi`] Skip all type definitions in `string-or-bytes-too-long (PYI053)`

---

_@dylwil3_

This PR skips all type definitions (including annotations, quoted annotations, etc.) for the purposes of [string-or-bytes-too-long (PYI053)](https://docs.astral.sh/ruff/rules/string-or-bytes-too-long/#string-or-bytes-too-long-pyi053).

Closes #12995 (we hope)
Replaces #13020 


---

_Label `rule` added by @dylwil3 on 2024-12-05 21:23_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-12-05 21:23_

---

_@dylwil3 reviewed on 2024-12-05 21:24_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:62 on 2024-12-05 21:24_

NB: All annotations are type definitions (but not conversely).

---

_Comment by @github-actions[bot] on 2024-12-05 21:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-06 00:34_

Thank you!!

---

_Merged by @dylwil3 on 2024-12-06 00:48_

---

_Closed by @dylwil3 on 2024-12-06 00:48_

---

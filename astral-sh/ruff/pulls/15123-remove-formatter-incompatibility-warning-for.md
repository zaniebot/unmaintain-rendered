```yaml
number: 15123
title: Remove formatter incompatibility warning for ISC001
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: ruff-0.9
head: micha/remove-isc001-warn
created_at: 2024-12-23T12:31:54Z
updated_at: 2025-01-03T13:24:26Z
url: https://github.com/astral-sh/ruff/pull/15123
synced_at: 2026-01-12T15:55:50Z
```

# Remove formatter incompatibility warning for ISC001

---

_@MichaReiser_

## Summary

Remove the formatter incompatility warning from `ISC001` because the formatter now:

* Tries to join implicit concatenated strings that fit on a single line
* Preserves implicit concatenated strings as mulitline for the few cases where the automatic-joining isn't supported


This PR adds a warning for `lint.flake8-implicit-str-concat.allow-multiline = false` if `ISC001` is disabled. 
This is not a new incompatibility, it's just that we forgot to add it. 

## Test Plan

See CLI tests


---

_Label `documentation` added by @MichaReiser on 2024-12-23 12:34_

---

_Comment by @github-actions[bot] on 2024-12-23 12:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-12-23 12:46_

---

_@charliermarsh approved on 2024-12-23 13:42_

---

_Merged by @MichaReiser on 2025-01-03 13:24_

---

_Closed by @MichaReiser on 2025-01-03 13:24_

---

_Branch deleted on 2025-01-03 13:24_

---

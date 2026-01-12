```yaml
number: 9944
title: Remove (legacy) formatter stability check from CI pipeline
type: pull_request
state: closed
author: MichaReiser
labels:
  - ci
assignees: []
base: main
head: remove-formatter-stability-check
created_at: 2024-02-12T09:29:14Z
updated_at: 2024-02-29T13:18:53Z
url: https://github.com/astral-sh/ruff/pull/9944
synced_at: 2026-01-12T15:55:30Z
```

# Remove (legacy) formatter stability check from CI pipeline

---

_@MichaReiser_

## Summary

We now have the ecosystem checks that gives us a better signal on formatting changes. 

It's still possible to run the check locally when necessary. 

## Test Plan

GitHub actions


---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-12 09:29_

---

_Label `ci` added by @MichaReiser on 2024-02-12 09:29_

---

_Comment by @github-actions[bot] on 2024-02-12 09:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Remove formatter stability check" to "Remove (legacy) formatter stability check from CI pipeline" by @MichaReiser on 2024-02-12 15:13_

---

_@zanieb approved on 2024-02-12 15:57_

Isn't the instability check important still? Do we have test coverage of this otherwise?

---

_Comment by @MichaReiser on 2024-02-12 16:45_

Oh, I wasn't aware that the script fails if there are stability or even syntax errors. I guess we have to keep it a little longer

---

_Closed by @MichaReiser on 2024-02-12 16:45_

---

_Branch deleted on 2024-02-29 13:18_

---

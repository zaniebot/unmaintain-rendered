```yaml
number: 12939
title: "[`ruff`] Ignore `fstring-missing-syntax` (`RUF027`) for `fastAPI` paths"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: alex/ruf027-fastapi
created_at: 2024-08-16T17:55:10Z
updated_at: 2024-08-17T10:10:36Z
url: https://github.com/astral-sh/ruff/pull/12939
synced_at: 2026-01-12T15:55:42Z
```

# [`ruff`] Ignore `fstring-missing-syntax` (`RUF027`) for `fastAPI` paths

---

_@AlexWaygood_

## Summary

As suggested by @MichaReiser in https://github.com/astral-sh/ruff/pull/12886#pullrequestreview-2237679793, this adds an exemption to `RUF027` for `fastAPI` paths, which require template strings rather than eagerly evaluated f-strings.

## Test Plan

I added a fixture that causes Ruff to emit a false-positive error on `main` but no longer does with this PR.


---

_Label `bug` added by @AlexWaygood on 2024-08-16 17:55_

---

_Label `preview` added by @AlexWaygood on 2024-08-16 17:55_

---

_Comment by @github-actions[bot] on 2024-08-16 18:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-17 06:35_

Nice!


---

_Merged by @AlexWaygood on 2024-08-17 10:10_

---

_Closed by @AlexWaygood on 2024-08-17 10:10_

---

_Branch deleted on 2024-08-17 10:10_

---

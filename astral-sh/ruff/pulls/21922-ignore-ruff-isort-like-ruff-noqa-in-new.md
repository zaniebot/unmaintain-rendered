```yaml
number: 21922
title: "Ignore ruff:isort like ruff:noqa in new suppressions"
type: pull_request
state: merged
author: amyreese
labels:
  - internal
  - preview
assignees: []
merged: true
base: main
head: amy/ruff-isort
created_at: 2025-12-11T16:27:23Z
updated_at: 2025-12-11T19:04:30Z
url: https://github.com/astral-sh/ruff/pull/21922
synced_at: 2026-01-10T16:42:11Z
```

# Ignore ruff:isort like ruff:noqa in new suppressions

---

_Pull request opened by @amyreese on 2025-12-11 16:27_

## Summary

Ignores `#ruff:isort` when parsing suppressions similar to `#ruff:noqa`.
Should clear up ecosystem issues in #21908

## Test Plan

cargo tests


---

_Comment by @astral-sh-bot[bot] on 2025-12-11 16:39_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser approved on 2025-12-11 16:49_

---

_Label `preview` added by @MichaReiser on 2025-12-11 16:49_

---

_Label `internal` added by @MichaReiser on 2025-12-11 16:49_

---

_Merged by @amyreese on 2025-12-11 19:04_

---

_Closed by @amyreese on 2025-12-11 19:04_

---

_Branch deleted on 2025-12-11 19:04_

---

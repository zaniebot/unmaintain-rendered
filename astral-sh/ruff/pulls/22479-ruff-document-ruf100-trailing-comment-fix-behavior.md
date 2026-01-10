```yaml
number: 22479
title: "[`ruff`] document `RUF100` trailing comment fix behavior"
type: pull_request
state: merged
author: Jkhall81
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix/ruff-100-docs-20762
created_at: 2026-01-09T13:46:57Z
updated_at: 2026-01-09T17:08:16Z
url: https://github.com/astral-sh/ruff/pull/22479
synced_at: 2026-01-10T15:56:07Z
```

# [`ruff`] document `RUF100` trailing comment fix behavior

---

_Pull request opened by @Jkhall81 on 2026-01-09 13:46_

## Summary
Ruff's `--fix` for `RUF100` can inadvertently remove trailing comments (e.g., `pylint` or `mypy` suppressions) by interpreting them as descriptions. This PR adds a "Conflict with other linters" section to the rule documentation to clarify this behavior and provide the double-hash (`# noqa # pylint`) workaround.

## Fixes
Fixes #20762

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 13:54_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `documentation` added by @amyreese on 2026-01-09 17:00_

---

_Renamed from "docs(ruff): document RUF100 trailing comment behavior (#20762)" to "[`ruff`] document `RUF100` trailing comment fix behavior" by @amyreese on 2026-01-09 17:00_

---

_Merged by @amyreese on 2026-01-09 17:01_

---

_Closed by @amyreese on 2026-01-09 17:01_

---

_Comment by @MichaReiser on 2026-01-09 17:08_

Makes sense. I'm surprised that pylint recognizes nested suppressions like this (without requiring `# pylint` 

---

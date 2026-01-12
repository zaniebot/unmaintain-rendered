```yaml
number: 19331
title: "[`flake8-use-pathlib`] Skip single dots for `invalid-pathlib-with-suffix` (`PTH210`) on versions >= 3.14"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - python314
assignees: []
merged: true
base: main
head: invalid-pthlib
created_at: 2025-07-14T15:37:26Z
updated_at: 2025-07-15T12:09:24Z
url: https://github.com/astral-sh/ruff/pull/19331
synced_at: 2026-01-12T15:56:37Z
```

# [`flake8-use-pathlib`] Skip single dots for `invalid-pathlib-with-suffix` (`PTH210`) on versions >= 3.14

---

_@dylwil3_

Skips [invalid-pathlib-with-suffix (PTH210)](https://docs.astral.sh/ruff/rules/invalid-pathlib-with-suffix/#invalid-pathlib-with-suffix-pth210) for `.with_suffix(".")` on Python versions 3.14 and greater, as per [the docs](https://docs.python.org/3.14/library/pathlib.html#pathlib.PurePath.with_suffix).

Progress towards #15506

---

_Label `rule` added by @dylwil3 on 2025-07-14 15:37_

---

_Label `python314` added by @dylwil3 on 2025-07-14 15:37_

---

_Comment by @github-actions[bot] on 2025-07-14 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__py314__PTH210_PTH210.py.snap`:1 on 2025-07-15 08:56_

I think it would be better to write a dedicated test for this behavior instead of copying the entire two files. For example: The errors header is inaccurate with Python 3.14 for `.with_suffix(".")`. It also feels pretty overkill (and maintaining those snapshots isn't free)

---

_@MichaReiser approved on 2025-07-15 08:56_

---

_Merged by @dylwil3 on 2025-07-15 12:05_

---

_Closed by @dylwil3 on 2025-07-15 12:05_

---

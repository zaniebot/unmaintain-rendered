```yaml
number: 14827
title: "[`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map` (`C417`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: walrus-compr
created_at: 2024-12-06T23:21:39Z
updated_at: 2024-12-07T03:06:00Z
url: https://github.com/astral-sh/ruff/pull/14827
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map` (`C417`)

---

_Pull request opened by @dylwil3 on 2024-12-06 23:21_

This PR modifies [unnecessary-map (C417)](https://docs.astral.sh/ruff/rules/unnecessary-map/#unnecessary-map-c417) to skip `map` expressions if the iterable contains a named expression, since those cannot appear in comprehensions.

Closes #14808 

---

_Comment by @github-actions[bot] on 2024-12-06 23:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-12-07 02:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:55 on 2024-12-07 02:12_

Can you add a tested for a nested expression in the target? Like `map(lambda x: x, [c := a])`

---

_@charliermarsh approved on 2024-12-07 02:12_

---

_Label `bug` added by @charliermarsh on 2024-12-07 02:13_

---

_Renamed from "[`flake8-comprehensions] Skip iterables with named expressions in `unnecessary-map (C417)`" to "[`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map (C417)`" by @charliermarsh on 2024-12-07 02:13_

---

_Renamed from "[`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map (C417)`" to "[`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map` (`C417`)" by @charliermarsh on 2024-12-07 02:13_

---

_@dylwil3 reviewed on 2024-12-07 02:54_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:55 on 2024-12-07 02:54_

whoops, I had meant to do something like that in the second example but nested the wrong thing. fixed now, thanks for catching that!

---

_Merged by @charliermarsh on 2024-12-07 03:00_

---

_Closed by @charliermarsh on 2024-12-07 03:00_

---

_Branch deleted on 2024-12-07 03:06_

---

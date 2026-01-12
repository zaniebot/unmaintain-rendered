```yaml
number: 14216
title: "[`refurb`] Further special cases added to `verbose-decimal-constructor (FURB157)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: decimal-redux
created_at: 2024-11-08T23:42:40Z
updated_at: 2024-11-09T04:05:23Z
url: https://github.com/astral-sh/ruff/pull/14216
synced_at: 2026-01-12T15:55:47Z
```

# [`refurb`] Further special cases added to `verbose-decimal-constructor (FURB157)`

---

_@dylwil3_

This PR accounts for further subtleties in `Decimal` parsing:

- Strings which are empty modulo underscores and surrounding whitespace are skipped
- `Decimal("-0")` is skipped
- `Decimal("{integer literal that is longer than 640 digits}")` are skipped (see linked issue for explanation)

NB: The snapshot did not need to be updated since the new test cases are "Ok" instances and added below the diff.

Closes #14204


---

_Comment by @github-actions[bot] on 2024-11-08 23:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-11-09 02:08_

---

_Label `bug` added by @charliermarsh on 2024-11-09 02:08_

---

_Merged by @charliermarsh on 2024-11-09 02:08_

---

_Closed by @charliermarsh on 2024-11-09 02:08_

---

_Branch deleted on 2024-11-09 04:05_

---

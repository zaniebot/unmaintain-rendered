```yaml
number: 16008
title: "[`flake8-pie`] Remove following comma correctly when the unpacked dictionary is empty (`PIE800`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: PIE800
created_at: 2025-02-07T00:30:59Z
updated_at: 2025-02-07T15:12:09Z
url: https://github.com/astral-sh/ruff/pull/16008
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-pie`] Remove following comma correctly when the unpacked dictionary is empty (`PIE800`)

---

_@InSyncWithFoo_

## Summary

Resolves #15997.

Ruff used to introduce syntax errors while fixing these cases, but no longer will:

```python
{"a": [], **{},}
#         ^^^^ Removed, leaving two contiguous commas

{"a": [], **({})}
#         ^^^^^ Removed, leaving a stray closing parentheses
```

Previously, the function would take a shortcut if the unpacked dictionary is empty; now, both cases are handled using the same logic introduced in #15394. This change slightly modifies that logic to also remove the first comma following the dictionary, if and only if it is empty.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-02-07 00:32_

The diff is best viewed with whitespace changes hidden.

---

_Comment by @github-actions[bot] on 2025-02-07 00:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-02-07 07:49_

---

_Label `fixes` added by @MichaReiser on 2025-02-07 07:49_

---

_@MichaReiser approved on 2025-02-07 07:51_

Thanks and hiding whitespace was an excellent suggestion!

---

_Merged by @MichaReiser on 2025-02-07 07:52_

---

_Closed by @MichaReiser on 2025-02-07 07:52_

---

_Branch deleted on 2025-02-07 15:12_

---

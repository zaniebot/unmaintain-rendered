```yaml
number: 13963
title: Regenerate known_stdlibs.rs with stdlibs 2024.10.25
type: pull_request
state: merged
author: thatch
labels:
  - rule
assignees: []
merged: true
base: main
head: stdlibs-2024.10.25
created_at: 2024-10-28T12:15:17Z
updated_at: 2024-10-28T12:38:10Z
url: https://github.com/astral-sh/ruff/pull/13963
synced_at: 2026-01-10T20:59:37Z
```

# Regenerate known_stdlibs.rs with stdlibs 2024.10.25

---

_Pull request opened by @thatch on 2024-10-28 12:15_

## Summary

`stdlibs` has a new release to properly categorize the `_wmi` module which has been [present since ~2022](https://github.com/python/cpython/issues/89545#issuecomment-1227846806).


## Test Plan

Let CI run, this is only a trivial change to categorization data.

---

_Comment by @github-actions[bot] on 2024-10-28 12:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-10-28 12:37_

---

_Merged by @charliermarsh on 2024-10-28 12:37_

---

_Closed by @charliermarsh on 2024-10-28 12:37_

---

_Label `rule` added by @charliermarsh on 2024-10-28 12:38_

---

```yaml
number: 9099
title: "E274: allow tab indentation before keyword"
type: pull_request
state: merged
author: sciyoshi
labels:
  - bug
assignees: []
merged: true
base: main
head: e274-ignore-indentation
created_at: 2023-12-11T20:12:32Z
updated_at: 2023-12-12T03:15:10Z
url: https://github.com/astral-sh/ruff/pull/9099
synced_at: 2026-01-12T15:55:27Z
```

# E274: allow tab indentation before keyword

---

_@sciyoshi_

## Summary

E274 currently flags any keyword at the start of a line indented with tabs. This turns out to be due to a bug in `Whitespace::trailing` that never considers any whitespace containing a tab as indentation.

## Test Plan

Added a simple test case.

---

_@charliermarsh approved on 2023-12-11 20:18_

---

_Label `bug` added by @charliermarsh on 2023-12-11 20:18_

---

_Comment by @charliermarsh on 2023-12-11 20:18_

Thanks!

---

_Comment by @github-actions[bot] on 2023-12-11 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-12-11 20:25_

---

_Closed by @charliermarsh on 2023-12-11 20:25_

---

_Branch deleted on 2023-12-12 03:15_

---

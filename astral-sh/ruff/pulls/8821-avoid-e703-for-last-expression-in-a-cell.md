```yaml
number: 8821
title: "Avoid `E703` for last expression in a cell"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/notebook-e703
created_at: 2023-11-22T17:19:32Z
updated_at: 2023-11-23T13:40:59Z
url: https://github.com/astral-sh/ruff/pull/8821
synced_at: 2026-01-12T15:55:27Z
```

# Avoid `E703` for last expression in a cell

---

_@dhruvmanila_

## Summary

This PR updates the `E703` rule to avoid flagging any semicolons if they're present after the last expression in a notebook cell. These are intended to hide the cell output.

Part of #8669 

## Test Plan

Add test notebook and update the snapshots.


---

_Label `rule` added by @dhruvmanila on 2023-11-22 17:19_

---

_@charliermarsh approved on 2023-11-22 17:27_

---

_Comment by @github-actions[bot] on 2023-11-22 17:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2023-11-23 13:40_

---

_Closed by @dhruvmanila on 2023-11-23 13:40_

---

_Branch deleted on 2023-11-23 13:40_

---

```yaml
number: 8302
title: "Remove `exclude` suggestion to use `extend-exclude` in `tool.ruff.format` config docs"
type: pull_request
state: merged
author: Giuzzilla
labels:
  - documentation
assignees: []
merged: true
base: main
head: format-exclude-remove-note
created_at: 2023-10-28T09:39:06Z
updated_at: 2023-10-28T13:08:51Z
url: https://github.com/astral-sh/ruff/pull/8302
synced_at: 2026-01-12T15:55:26Z
```

# Remove `exclude` suggestion to use `extend-exclude` in `tool.ruff.format` config docs

---

_@Giuzzilla_

## Summary

Remove wrong note on `tool.ruff.format` `exclude` option from documentation which is referencing `extend-exclude` even if it's not relevant for the formatter options (`exclude` is additive). See #8301 

## Test Plan

N/A (Docs change)

---

_Comment by @github-actions[bot] on 2023-10-28 09:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-10-28 11:15_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-10-28 11:15_

---

_Merged by @charliermarsh on 2023-10-28 11:15_

---

_Closed by @charliermarsh on 2023-10-28 11:15_

---

_Branch deleted on 2023-10-28 13:08_

---

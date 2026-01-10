```yaml
number: 11513
title: "Use `Importer` available on `Checker`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/checker-importer
created_at: 2024-05-23T11:15:23Z
updated_at: 2024-05-23T11:28:53Z
url: https://github.com/astral-sh/ruff/pull/11513
synced_at: 2026-01-10T22:05:26Z
```

# Use `Importer` available on `Checker`

---

_Pull request opened by @dhruvmanila on 2024-05-23 11:15_

## Summary

This PR updates the `FA102` rule logic to use the `Importer` which is available on the `Checker`.

The main motivation is that this would make updating the `Importer` to use the `Tokens` struct which will be required to remove the `lex_starts_at` usage in `Insertion::start_of_block` method.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-05-23 11:15_

---

_Merged by @dhruvmanila on 2024-05-23 11:19_

---

_Closed by @dhruvmanila on 2024-05-23 11:19_

---

_Branch deleted on 2024-05-23 11:19_

---

_Comment by @github-actions[bot] on 2024-05-23 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

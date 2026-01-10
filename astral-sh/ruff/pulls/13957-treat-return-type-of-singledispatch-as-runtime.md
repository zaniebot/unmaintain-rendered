```yaml
number: 13957
title: "Treat return type of `singledispatch` as runtime-required"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/tch003-return-type
created_at: 2024-10-28T05:12:26Z
updated_at: 2024-10-29T00:33:30Z
url: https://github.com/astral-sh/ruff/pull/13957
synced_at: 2026-01-10T20:59:37Z
```

# Treat return type of `singledispatch` as runtime-required

---

_Pull request opened by @dhruvmanila on 2024-10-28 05:12_

## Summary

fixes: #13955 

## Test Plan

Update existing test case to use a return type hint for which `main` flags `TCH003`.


---

_Label `bug` added by @dhruvmanila on 2024-10-28 05:12_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-10-28 05:12_

---

_Comment by @github-actions[bot] on 2024-10-28 06:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-10-28 07:52_

The code changes look good to me. Let's wait for @AlexWaygood to confirm the semantics.

---

_Comment by @charliermarsh on 2024-10-28 12:41_

I think this is right.

---

_Merged by @charliermarsh on 2024-10-29 00:33_

---

_Closed by @charliermarsh on 2024-10-29 00:33_

---

_Branch deleted on 2024-10-29 00:33_

---

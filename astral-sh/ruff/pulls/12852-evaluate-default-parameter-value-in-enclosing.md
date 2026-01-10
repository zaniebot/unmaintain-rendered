```yaml
number: 12852
title: Evaluate default parameter value in enclosing scope
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/default-param
created_at: 2024-08-13T06:14:17Z
updated_at: 2024-08-13T13:55:52Z
url: https://github.com/astral-sh/ruff/pull/12852
synced_at: 2026-01-10T21:38:32Z
```

# Evaluate default parameter value in enclosing scope

---

_Pull request opened by @dhruvmanila on 2024-08-13 06:14_

## Summary

This PR fixes a bug in the semantic model where it would evaluate the default parameter value in the type parameter scope. For example,

```py
def foo[T1: int](a = T1):
    pass
```

Here, the `T1` in `a = T1` is undefined but Ruff doesn't flag it (https://play.ruff.rs/ba2f7c2f-4da6-417e-aa2a-104aa63e6d5e).

The fix here is to evaluate the default parameter value in the _enclosing_ scope instead.

## Test Plan

Add a test case which includes the above code under `F821` (`undefined-name`) and validate the snapshot.

---

_Label `bug` added by @dhruvmanila on 2024-08-13 06:14_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-13 06:14_

---

_Comment by @github-actions[bot] on 2024-08-13 06:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-13 11:25_

Nice catch, LGTM!

---

_Merged by @dhruvmanila on 2024-08-13 13:55_

---

_Closed by @dhruvmanila on 2024-08-13 13:55_

---

_Branch deleted on 2024-08-13 13:55_

---

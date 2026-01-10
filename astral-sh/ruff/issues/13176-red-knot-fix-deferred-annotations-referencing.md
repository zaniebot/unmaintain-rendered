```yaml
number: 13176
title: "[red-knot] fix deferred annotations referencing other scopes"
type: issue
state: closed
author: carljm
labels:
  - bug
  - ty
assignees: []
created_at: 2024-08-30T23:42:33Z
updated_at: 2024-09-04T17:10:55Z
url: https://github.com/astral-sh/ruff/issues/13176
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] fix deferred annotations referencing other scopes

---

_Issue opened by @carljm on 2024-08-30 23:42_

Our current handling for resolving deferred annotations uses the inferred types of symbols as they exist at the end of the current scope (rather than the current point in control flow of the current scope), which is correct (or at least the most reasonable option) since their resolution is deferred.

But it currently does this by piggy-backing on the same logic we use for e.g. import resolution, where the name must exist in the scope's namespace, there's no fallback to other scopes. So we can't resolve a deferred annotation that's a builtin, or a global or nonlocal referred to from a nested scope. This should be fixed.

---

_Label `red-knot` added by @carljm on 2024-08-30 23:42_

---

_Label `bug` added by @carljm on 2024-08-30 23:42_

---

_Closed by @carljm on 2024-09-04 17:10_

---

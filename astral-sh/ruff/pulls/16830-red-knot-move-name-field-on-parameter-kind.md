```yaml
number: 16830
title: "[red-knot] Move `name` field on parameter kind"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/parameter-name
created_at: 2025-03-18T14:22:45Z
updated_at: 2025-03-18T17:17:46Z
url: https://github.com/astral-sh/ruff/pull/16830
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Move `name` field on parameter kind

---

_@dhruvmanila_

## Summary

Previously, the `name` field was on `Parameter` which required it to be always optional regardless of the parameter kind because a `typing.Callable` signature does not have name for the parameters. This is the case for positional-only parameters. This wasn't enforced at the type level which meant that downstream usages would have to unwrap on `name` even though it's guaranteed to be present.

This commit moves the `name` field from `Parameter` to the `ParameterKind` variants and makes it optional only for `ParameterKind::PositionalOnly` variant while required for all other variants.

One change that's now required is that a `Callable` form using a gradual form for parameter types (`...`) would have a default `args` and `kwargs` name used for variadic and keyword-variadic parameter kind respectively. This is also the case for invalid `Callable` type forms. I think this is fine as names are not relevant in this context but happy to make it optional even in variadic variants.

## Test Plan

No new tests; make sure existing tests are passing.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-18 14:22_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-18 14:22_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-18 14:22_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-18 14:22_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-18 14:22_

---

_Comment by @github-actions[bot] on 2025-03-18 14:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-03-18 14:36_

Looks good!

---

_@dcreager approved on 2025-03-18 15:20_

---

_Merged by @dhruvmanila on 2025-03-18 17:17_

---

_Closed by @dhruvmanila on 2025-03-18 17:17_

---

_Branch deleted on 2025-03-18 17:17_

---

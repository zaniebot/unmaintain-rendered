```yaml
number: 17107
title: "[red-knot] support Any as a class in typeshed"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/anyclass
created_at: 2025-04-01T01:27:48Z
updated_at: 2025-04-01T16:38:25Z
url: https://github.com/astral-sh/ruff/pull/17107
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] support Any as a class in typeshed

---

_Pull request opened by @carljm on 2025-04-01 01:27_

## Summary

In https://github.com/python/typeshed/pull/13520 the typeshed definition of `typing.Any` was changed from `Any = object()` to `class Any: ...`. Our automated typeshed updater pulled down this change in https://github.com/astral-sh/ruff/pull/17106, with the consequence that we no longer understand `Any`, which is... not good.

This PR gives us the ability to understand `Any` defined as a class instead of `object()`. It doesn't remove our ability to understand the old form. Perhaps at some point we'll want to remove it, but for now we may as well support both old and new typeshed?

This also directly patches typeshed to use the new form of `Any`; this is purely to work around our tests that no known class is inferred as `Unknown`, which otherwise fail with the old typeshed and the changes in this PR. (All other tests pass.) This patch to typeshed will shortly be subsumed by https://github.com/astral-sh/ruff/pull/17106 anyway.

## Test Plan

Without the typeshed change in this PR, all tests pass except for the two `known_class_doesnt_fallback_to_unknown_unexpectedly_*` tests (so we still support the old form of defining `Any`). With the typeshed change in this PR, all tests pass, so we now support the new form in a way that is indistinguishable to our test suite from the old form. And indistinguishable to the ecosystem check: after rebasing https://github.com/astral-sh/ruff/pull/17106 on this PR, there's zero ecosystem impact.




---

_Label `red-knot` added by @carljm on 2025-04-01 01:27_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-01 01:27_

---

_Review requested from @sharkdp by @carljm on 2025-04-01 01:27_

---

_Review requested from @dcreager by @carljm on 2025-04-01 01:27_

---

_Review requested from @MichaReiser by @carljm on 2025-04-01 01:27_

---

_Comment by @github-actions[bot] on 2025-04-01 01:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-01 07:20_

---

_Comment by @MichaReiser on 2025-04-01 07:22_

Should we add a TODO or at least a comment to the `Any` (object) to clarify that this is no longer used with recent typeshed versions?

---

_Merged by @carljm on 2025-04-01 16:38_

---

_Closed by @carljm on 2025-04-01 16:38_

---

_Branch deleted on 2025-04-01 16:38_

---

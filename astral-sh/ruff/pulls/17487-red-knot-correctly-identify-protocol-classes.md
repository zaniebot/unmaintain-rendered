```yaml
number: 17487
title: "[red-knot] Correctly identify protocol classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/is-protocol
created_at: 2025-04-19T21:44:37Z
updated_at: 2025-04-21T15:17:10Z
url: https://github.com/astral-sh/ruff/pull/17487
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Correctly identify protocol classes

---

_Pull request opened by @AlexWaygood on 2025-04-19 21:44_

## Summary

A protocol class is one that has either `Protocol` or `Protocol[]` in its class bases. Fix our recognition of whether or not a class is a protocol class, and allow this to be introspected in mdtests via the helper function `typing(_extensions).is_protocol()`.

## Test Plan

mdtests added and updated.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-19 21:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-19 21:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-19 21:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-19 21:44_

---

_Comment by @github-actions[bot] on 2025-04-19 21:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager approved on 2025-04-21 13:58_

---

_Merged by @AlexWaygood on 2025-04-21 15:17_

---

_Closed by @AlexWaygood on 2025-04-21 15:17_

---

_Branch deleted on 2025-04-21 15:17_

---

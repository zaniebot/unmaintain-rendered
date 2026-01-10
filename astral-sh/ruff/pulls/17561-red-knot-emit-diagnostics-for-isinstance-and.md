```yaml
number: 17561
title: "[red-knot] Emit diagnostics for isinstance() and issubclass() calls where a non-runtime-checkable protocol is the second argument"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/runtime-checkable-protos
created_at: 2025-04-22T18:35:18Z
updated_at: 2025-04-23T21:45:20Z
url: https://github.com/astral-sh/ruff/pull/17561
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Emit diagnostics for isinstance() and issubclass() calls where a non-runtime-checkable protocol is the second argument

---

_Pull request opened by @AlexWaygood on 2025-04-22 18:35_

## Summary

This PR adds diagnostics for `isinstance()` and `issubclass()` calls where the second argument is a protocol class that is not declared as runtime-checkable. Using @BurntSushi's new diagnostics infrastructure, it makes these diagnostics _beautiful_ too! Here's a screenshot of what they look like in the terminal:

![image](https://github.com/user-attachments/assets/4c0ecdf3-f79f-40bc-8f3f-f76ad665617e)

## Test Plan

Existing mdtests updated, with new snapshots.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-22 18:35_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-22 18:35_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-22 18:35_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-22 18:35_

---

_@AlexWaygood reviewed on 2025-04-22 18:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/protocols.md_-_Protocols_-_Narrowing_of_protocols.snap`:1 on 2025-04-22 18:38_

This snapshot honestly feels much too big to me. I only really want to snapshot the new diagnostics for `isinstance()` and `issubclass()`; all the `reveal_type` diagnostics being snapshotted here are pretty noisy. But in order to get a smaller snapshot, I'd have to add another subheading to the `protocols.md` file, which would break the flow of my literate test suite ☹️

---

_Comment by @github-actions[bot] on 2025-04-22 18:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:1255 on 2025-04-23 04:19_

Why should this be an error? It seems to satisfy both conditions mentioned above.

---

_@carljm approved on 2025-04-23 04:35_

Cool!

---

_@AlexWaygood reviewed on 2025-04-23 12:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:1255 on 2025-04-23 12:00_

whoooops, that's a bug in the test!

---

_Merged by @AlexWaygood on 2025-04-23 21:40_

---

_Closed by @AlexWaygood on 2025-04-23 21:40_

---

_Branch deleted on 2025-04-23 21:40_

---

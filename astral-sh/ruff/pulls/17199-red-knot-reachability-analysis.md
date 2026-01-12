```yaml
number: 17199
title: "[red-knot] Reachability analysis"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/silence-unresolved-reference-in-unreachable-code-pt2
created_at: 2025-04-04T11:11:53Z
updated_at: 2025-04-08T06:37:22Z
url: https://github.com/astral-sh/ruff/pull/17199
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Reachability analysis

---

_@sharkdp_

## Summary

This PR proposes a new approach to silencing `unresolved-reference` diagnostics by keeping track of the reachability of each use of a symbol. The changes merged in https://github.com/astral-sh/ruff/pull/17169 are still needed for the "Use of variable in nested function" test case, but that could also be solved in another way eventually (see https://github.com/astral-sh/ty/issues/210). We can use the same technique to silence `unresolved-import` and `unresolved-attribute` false-positives, but I think this could be merged in isolation.

## Test Plan

New Markdown tests, ecosystem tests

---

_Label `red-knot` added by @sharkdp on 2025-04-04 11:11_

---

_Comment by @github-actions[bot] on 2025-04-04 11:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:28:22: Name `ExceptionGroup` used when not defined
- Found 96 diagnostics
+ Found 95 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:64:36: Name `TemporaryFile` used when possibly not defined
- Found 683 diagnostics
+ Found 682 diagnostics

rich (https://github.com/Textualize/rich)
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/rich/traceback.py:462:43: Name `BaseExceptionGroup` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/rich/traceback.py:462:63: Name `ExceptionGroup` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_progress.py:186:13: Name `_time` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_progress.py:499:20: Name `time` used when not defined
- Found 382 diagnostics
+ Found 378 diagnostics

```
</details>


---

_@sharkdp reviewed on 2025-04-04 18:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:620 on 2025-04-04 18:39_

I'm completely fine with keeping this more concise.

---

_@sharkdp reviewed on 2025-04-04 18:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:433 on 2025-04-04 18:39_

I will leave attributes and imports for a follow up. I want to get some feedback on this first.

---

_Marked ready for review by @sharkdp on 2025-04-04 18:47_

---

_Review requested from @carljm by @sharkdp on 2025-04-04 18:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-04 18:47_

---

_Review requested from @dcreager by @sharkdp on 2025-04-04 18:47_

---

_@sharkdp reviewed on 2025-04-04 19:29_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:221 on 2025-04-04 19:29_

Nothing changed in this test, but this explanation was outdated. And the `return x  # …` below as confusing.

---

_@sharkdp reviewed on 2025-04-04 19:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:265 on 2025-04-04 19:30_

The fact that we reveal `Unknown` here is tracked in https://github.com/astral-sh/ruff/issues/15777

---

_@sharkdp reviewed on 2025-04-04 19:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:275 on 2025-04-04 19:31_

We don't support `Final` yet, so I needed to declare this as `Literal[False]`. Otherwise, the public type (which we use below) would be `Unknown | Literal[False]`.

---

_@sharkdp reviewed on 2025-04-04 20:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4191 on 2025-04-04 20:31_

We could potentially defer the evaluation of the visibility constraint here, if it turns out to be a performance problem.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:894 on 2025-04-08 02:46_

nit: we should also `self.reachability_by_use.shrink_to_fit()` above?

---

_@carljm approved on 2025-04-08 02:49_

Looks good to me!

---

_Merged by @sharkdp on 2025-04-08 06:37_

---

_Closed by @sharkdp on 2025-04-08 06:37_

---

_Branch deleted on 2025-04-08 06:37_

---

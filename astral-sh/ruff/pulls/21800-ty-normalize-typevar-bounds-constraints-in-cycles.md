```yaml
number: 21800
title: "[ty] normalize typevar bounds/constraints in cycles"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/boundcycle
created_at: 2025-12-04T21:55:06Z
updated_at: 2025-12-04T23:17:59Z
url: https://github.com/astral-sh/ruff/pull/21800
synced_at: 2026-01-12T15:57:34Z
```

# [ty] normalize typevar bounds/constraints in cycles

---

_@carljm_

Fixes https://github.com/astral-sh/ty/issues/1587

## Summary

Perform cycle normalization on typevar bounds and constraints (similar to how it was already done for typevar defaults) in order to ensure convergence in cyclic cases.

There might be another fix here that could avoid the cycle in many more cases, where we don't eagerly evaluate typevar bounds/constraints on explicit specialization, but just accept the given specialization and later evaluate to see whether we need to emit a diagnostic on it. But the current fix here is sufficient to solve the problem and matches the patterns we use to ensure cycle convergence elsewhere, so it seems good for now; left a TODO for the other idea.

This fix is sufficient to make us not panic, but not sufficient to get the semantics fully correct; see the TODOs in the tests. I have ideas for fixing that as well, but it seems worth at least getting this in to fix the panic.

## Test Plan

Test that previously panicked now does not.


---

_Label `ty` added by @carljm on 2025-12-04 21:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 21:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 21:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 39 diagnostics
+ Found 38 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/testing/internal/test_data.py:140:15: warning[unsupported-base] Unsupported class base with type `<class 'TestItem[Test, Never]'> | <class 'TestItem[Unknown, Unknown]'>`
- Found 8325 diagnostics
+ Found 8326 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:1041:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2052 diagnostics
+ Found 2050 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @carljm on 2025-12-04 22:01_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-04 22:01_

---

_Review requested from @sharkdp by @carljm on 2025-12-04 22:01_

---

_Review requested from @dcreager by @carljm on 2025-12-04 22:01_

---

_@dcreager approved on 2025-12-04 22:41_

👍 to landing this separately from any follow-on work

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9735 on 2025-12-04 22:49_

```suggestion
#[expect(clippy::ref_option)]
```

---

_@AlexWaygood approved on 2025-12-04 22:49_

---

_@carljm reviewed on 2025-12-04 23:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9751 on 2025-12-04 23:03_

```suggestion
#[expect(clippy::ref_option)]
```

---

_Merged by @carljm on 2025-12-04 23:17_

---

_Closed by @carljm on 2025-12-04 23:17_

---

_Branch deleted on 2025-12-04 23:17_

---

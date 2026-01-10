```yaml
number: 17731
title: "[red-knot] Fix recording of negative visibility constraints"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-negative-reachability-constraints
created_at: 2025-04-30T07:15:40Z
updated_at: 2025-04-30T13:43:19Z
url: https://github.com/astral-sh/ruff/pull/17731
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Fix recording of negative visibility constraints

---

_Pull request opened by @sharkdp on 2025-04-30 07:15_

## Summary

We were previously recording wrong reachability constraints for negative branches. Instead of `[cond] AND (NOT [True])` below, we were recording `[cond] AND (NOT ([cond] AND [True]))`, i.e. we were negating not just the last predicate, but the `AND`-ed reachability constraint from last clause. With this fix, we now record the correct constraints for the example from #17723:

```py
def _(cond: bool):
    if cond:
        # reachability: [cond]
        if True:
            # reachability: [cond] AND [True]
            pass
        else:
            # reachability: [cond] AND (NOT [True])
            x
```

closes #17723 

## Test Plan

* Regression test.
* Verified the ecosystem changes

---

_Label `red-knot` added by @sharkdp on 2025-04-30 07:15_

---

_Comment by @github-actions[bot] on 2025-04-30 07:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- warning[lint:unresolved-reference] kornia/utils/helpers.py:302:35: Name `A_LU` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:302:41: Name `pivots` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:305:41: Name `A_LU` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:305:47: Name `pivots` used when not defined
- Found 1014 diagnostics
+ Found 1010 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- error[lint:unresolved-attribute] pyi.py:2043:31: Type `<module 'ast'>` has no attribute `TypeVar`
- Found 33 diagnostics
+ Found 32 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- warning[lint:unresolved-reference] src/aiortc/rtcsctptransport.py:530:31: Name `expected_tsn` used when not defined
- warning[lint:unresolved-reference] src/aiortc/rtcsctptransport.py:531:20: Name `ordered` used when not defined
- Found 131 diagnostics
+ Found 129 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:4527:20: Type `<module 'typing'>` has no attribute `_eval_type`
- error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:4534:16: Type `<module 'typing'>` has no attribute `_eval_type`
- Found 1244 diagnostics
+ Found 1242 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- warning[lint:unresolved-reference] pyodide-build/pyodide_build/pywasmcross.py:332:9: Name `symbol_lines` used when not defined
- warning[lint:unresolved-reference] pyodide-build/pyodide_build/pywasmcross.py:342:68: Name `symbol_lines` used when not defined
- Found 1258 diagnostics
+ Found 1256 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- warning[lint:unresolved-reference] lib/Crypto/SelfTest/Hash/test_BLAKE2.py:293:42: Name `input_data` used when not defined
- warning[lint:unresolved-reference] lib/Crypto/SelfTest/Hash/test_BLAKE2.py:293:54: Name `key` used when not defined
- Found 2184 diagnostics
+ Found 2182 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[lint:possibly-unresolved-reference] static_frame/core/util.py:1269:32: Name `rows` used when possibly not defined
- Found 4764 diagnostics
+ Found 4763 diagnostics

jax (https://github.com/google/jax)
- error[lint:unresolved-attribute] jax/_src/export/_export.py:763:41: Type `None` has no attribute `_aval`
- warning[lint:unresolved-reference] jax/_src/interpreters/ad.py:284:67: Name `aux` used when not defined
- Found 6082 diagnostics
+ Found 6080 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-30 07:30_

---

_Review requested from @carljm by @sharkdp on 2025-04-30 07:30_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-30 07:30_

---

_Review requested from @dcreager by @sharkdp on 2025-04-30 07:30_

---

_Comment by @sharkdp on 2025-04-30 07:32_

Merging this without review, as the fix is relatively obvious in hindsight (after two hours of debugging :cry:).

---

_Merged by @sharkdp on 2025-04-30 07:32_

---

_Closed by @sharkdp on 2025-04-30 07:32_

---

_Branch deleted on 2025-04-30 07:32_

---

_@dcreager reviewed on 2025-04-30 13:43_

:+1: post-merge review looks good!

---

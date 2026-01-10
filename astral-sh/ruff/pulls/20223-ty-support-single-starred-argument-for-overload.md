```yaml
number: 20223
title: "[ty] Support single-starred argument for overload call"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/skip-filtering
created_at: 2025-09-04T08:08:54Z
updated_at: 2025-10-02T14:41:58Z
url: https://github.com/astral-sh/ruff/pull/20223
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Support single-starred argument for overload call

---

_Pull request opened by @dhruvmanila on 2025-09-04 08:08_

## Summary

closes: https://github.com/astral-sh/ty/issues/247

This PR adds support for variadic arguments to overload call evaluation.

This basically boils down to making sure that the overloads are not filtered out incorrectly during the step 5 in the overload call evaluation algorithm. For context, the step 5 tries to filter out the remaining overloads after finding an overload where the materialization of argument types are assignable to the parameter types.

The issue with the previous implementation was that it wouldn't unpack the variadic argument and wouldn't consider the many-to-one (multiple arguments mapping to a single variadic parameter) correctly. This PR fixes that.

## Test Plan

Update existing test cases and resolve the TODOs.


---

_Label `ty` added by @dhruvmanila on 2025-09-04 08:08_

---

_Comment by @github-actions[bot] on 2025-09-04 08:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-04 08:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_dunders.py:677:15: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_dunders.py:677:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- tests/test_make.py:2169:15: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2169:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- tests/test_make.py:2191:22: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2191:22: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- tests/test_make.py:2192:30: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2192:30: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/git_right_click.py:56:22: error[unresolved-attribute] Type `str` has no attribute `decode`
- porcupine/plugins/git_right_click.py:57:24: error[unresolved-attribute] Type `str` has no attribute `decode`
- Found 26 diagnostics
+ Found 24 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/dbg/gdb/__init__.py:1750:9: error[invalid-assignment] Object of type `str` is not assignable to `Literal["att", "intel"]`
- Found 2571 diagnostics
+ Found 2572 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-kubernetes/prefect_kubernetes/worker.py:627:5: error[invalid-assignment] Object of type `Literal[KubernetesImagePullPolicy.IF_NOT_PRESENT]` is not assignable to `Literal["IfNotPresent", "Always", "Never"]`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:109:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:141:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:156:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:256:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:287:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:302:5: error[invalid-assignment] Object of type `None` is not assignable to `str`
- Found 3180 diagnostics
+ Found 3187 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/sonos/diagnostics.py:129:20: error[invalid-return-type] Return type does not match returned value: expected `int | float | str | dict[str, Any]`, found `MappingProxyType[str, Any]`
- Found 13733 diagnostics
+ Found 13732 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Skip overload filtering if there are no participating parameters" to "[ty] Support single-starred argument for overload call" by @dhruvmanila on 2025-09-19 10:40_

---

_Comment by @codspeed-hq[bot] on 2025-09-29 11:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fskip-filtering?runnerMode=WallTime)

### Merging #20223 will **not alter performance**

<sub>Comparing <code>dhruv/skip-filtering</code> (58d4d9f) with <code>main</code> (8664842)</sub>



### Summary

`✅ 8` untouched  





---

_Comment by @codspeed-hq[bot] on 2025-09-29 11:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fskip-filtering?runnerMode=Instrumentation)

### Merging #20223 will **not alter performance**

<sub>Comparing <code>dhruv/skip-filtering</code> (58d4d9f) with <code>main</code> (8664842)</sub>



### Summary

`✅ 13` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fskip-filtering?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Comment by @dhruvmanila on 2025-09-29 17:15_

~I plan on looking at the performance regression tomorrow morning but otherwise I think this is ready to get some initial feedback.~

It seems like it was just noise.

---

_Marked ready for review by @dhruvmanila on 2025-09-29 17:15_

---

_Review requested from @carljm by @dhruvmanila on 2025-09-29 17:15_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-09-29 17:15_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-09-29 17:15_

---

_Review requested from @dcreager by @dhruvmanila on 2025-09-29 17:15_

---

_@dcreager approved on 2025-10-02 00:57_

Looks good!

---

_Merged by @dcreager on 2025-10-02 14:41_

---

_Closed by @dcreager on 2025-10-02 14:41_

---

_Branch deleted on 2025-10-02 14:41_

---

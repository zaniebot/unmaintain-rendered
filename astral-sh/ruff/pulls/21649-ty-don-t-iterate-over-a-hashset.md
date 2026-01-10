```yaml
number: 21649
title: "[ty] don't iterate over a hashset"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/nondet
created_at: 2025-11-26T23:09:09Z
updated_at: 2025-11-27T06:45:40Z
url: https://github.com/astral-sh/ruff/pull/21649
synced_at: 2026-01-10T16:48:02Z
```

# [ty] don't iterate over a hashset

---

_Pull request opened by @carljm on 2025-11-26 23:09_

## Summary

This caused "deterministic but chaotic" ordering of some intersection types in diagnostics. When calling a union, we infer the argument type once per matching parameter type, intersecting the inferred types for the argument expression, and we did that in an unpredictable order.

We do need a hashset here for de-duplication. Sometimes we call large unions where the type for a given parameter is the same across the union, we should infer the argument once per parameter type, not once per union element. So use an `FxIndexSet` instead of an `FxHashSet`.

## Test Plan

With this change, switching between `main` and https://github.com/astral-sh/ruff/pull/21646 no longer changes the ordering of the intersection type in the test in https://github.com/astral-sh/ruff/pull/21646/commits/cca3a8045df3a1038601e848b87b2163c81aebed


---

_Label `ty` added by @carljm on 2025-11-26 23:09_

---

_Marked ready for review by @carljm on 2025-11-26 23:09_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-26 23:09_

---

_Review requested from @sharkdp by @carljm on 2025-11-26 23:09_

---

_Review requested from @dcreager by @carljm on 2025-11-26 23:09_

---

_Review requested from @ibraheemdev by @carljm on 2025-11-26 23:10_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 23:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-26 23:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[tuple[str, str, str] | tuple[str, int, Unknown]] & list[Unknown | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @ibraheemdev on 2025-11-26 23:14_

Good catch, thanks.

We could simplify this a bit further (but up to you).
```diff
- let parameter_types = overloads_with_binding
-     .iter()
-     .filter_map(|(overload, binding)| parameter_type(overload, binding));
- for parameter_type in parameter_types {
+ for (overload, binding) in overloads_with_bindings {
+     let parameter_type = parameter_type(overload, binding);
```

---

_Comment by @ibraheemdev on 2025-11-26 23:15_

Hmm.. the mypy-primer diagnostic changes are somewhat concerning.

---

_@ibraheemdev approved on 2025-11-26 23:15_

---

_Comment by @carljm on 2025-11-26 23:17_

> We could simplify this a bit further (but up to you).

I think we can't do that, because of the `filter_map` (`parameter_type(...)` returns an option). Or, we can, but it means adding a conditional `continue`, or `if let` and extra indentation level, in the loop, not sure that's really better.

---

_Comment by @AlexWaygood on 2025-11-26 23:18_

> Hmm.. the mypy-primer diagnostic changes are somewhat concerning.

I'm also seeing them on https://github.com/astral-sh/ruff/pull/21644, which only touches documentation. I think there's been more than one new cause of nondeterminism over the last few days :-) it looks related to https://github.com/astral-sh/ruff/pull/20566

---

_Comment by @carljm on 2025-11-26 23:19_

Yes, I don't think this fixes the only issue.

The dd-trace-py change looks like this PR, the other changes less so.

---

_Comment by @codspeed-hq[bot] on 2025-11-26 23:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnondet?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21649 will **not alter performance**

<sub>Comparing <code>cjm/nondet</code> (68647e2) with <code>main</code> (e2e2150)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnondet?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@AlexWaygood approved on 2025-11-27 00:18_

---

_Merged by @carljm on 2025-11-27 00:39_

---

_Closed by @carljm on 2025-11-27 00:39_

---

_Branch deleted on 2025-11-27 00:39_

---

_@MichaReiser reviewed on 2025-11-27 06:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6904 on 2025-11-27 06:45_

It would probably have been worth to adding a comment here or someone like me will come along and change this to a hash set again 😆 

---

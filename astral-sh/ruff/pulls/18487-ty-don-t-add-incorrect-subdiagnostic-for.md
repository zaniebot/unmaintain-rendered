```yaml
number: 18487
title: "[ty] Don't add incorrect subdiagnostic for unresolved reference"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - bug
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: incorrect-subdiagnostic-for-unresolved-reference
created_at: 2025-06-05T21:22:28Z
updated_at: 2025-06-28T23:12:52Z
url: https://github.com/astral-sh/ruff/pull/18487
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Don't add incorrect subdiagnostic for unresolved reference

---

_Pull request opened by @MatthewMckee4 on 2025-06-05 21:22_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Don't add subdiagnostic in staticmethod, and change message for classmethod

Resolves https://github.com/astral-sh/ty/issues/584 (partially i think)

## Test Plan

Add test and update snapshot


---

_Review requested from @carljm by @MatthewMckee4 on 2025-06-05 21:22_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-06-05 21:22_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-06-05 21:22_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-06-05 21:22_

---

_Comment by @github-actions[bot] on 2025-06-05 21:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @ntBre on 2025-06-05 21:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1001 on 2025-06-05 23:23_

I think the other thing to test in a classmethod is that we don't suggest anything at all if the variable name matches an instance-only attribute. E.g. if we add an `__init__` method below that has `self.y = 1` and then reference `y` in the classmethod, we should not suggest `cls.y` (or anything at all) for `y`

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1823 on 2025-06-05 23:32_

I think it would be a bit nicer if this method instead returned a `FunctionDecorators` bitflag, rather than allocating a vector and requiring callers to sometimes clone it, and do all the type inference etc themselves.

Extra good would be if we could consolidate with the code that currently lives in `TypeInferenceBuilder::infer_function_definition` so we just have one occurrence of the match statement to build a `FunctionDecorators` bitflag.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:6154 on 2025-06-05 23:33_

Not sure if you were intending to leave this for a later PR (if so a test with TODO comment would still be good), but @AlexWaygood 's point in the linked issue was that we should not hardcode either the name `self` or the name `cls` based on what kind of method it is -- rather we should use the actual name of the actual first parameter of the method. It's perfectly valid Python to use a name other than `self` as first parameter of a regular method, or a name other than `cls` as first parameter of a classmethod. (We should also have tests for these cases.)

---

_@carljm had review dismissed on 2025-06-05 23:34_

Thank you!

---

_@AlexWaygood reviewed on 2025-06-06 10:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1823 on 2025-06-06 10:47_

Another issue here is that this method is also called `current_method_decorators()`, but I don't see any checks in the method that the function scope is nested inside a class scope. It looks like it will return `Some()` for any function scope, not just method scopes. So maybe it should be called `current_function_decorators()`?

---

_Comment by @MatthewMckee4 on 2025-06-09 12:40_

I've cleaned this up a bit, but i'm getting panics because of the decorator expression inference. Im not sure what order and function i should call to infer the type of the decorator expression.

---

_Comment by @carljm on 2025-06-10 20:53_

A core invariant of our type inference is that each AST node gets a type inferred for it once, in the "inference region" responsible for that node (an inference region can be a scope, a definition, or a standalone expression), and then that type gets merged into the surrounding region as needed, but doesn't get re-inferred willy-nilly.

So I don't think it will work well to have the `function_decorators` method just take a list of AST nodes; this means we'll end up re-inferring its type ad-hoc from various call-sites, and that won't work well.

The function decorators should have their type inferred exactly once, in `infer_function_definition` (via `infer_decorator`). We shouldn't call `self.infer_expression` or similar on those same nodes anywhere else.

The previous code here that checked for abstract methods and overloads used `self.file_expression_type` on those decorator nodes, which doesn't re-infer them: it looks up what scope they belong to, infers that entire scope, and then pulls the expression types out. This worked, but isn't really ideal because it infers far more than necessary. A function definition is always a definition, and the decorator nodes are part of that definition, so we don't need to infer the entire scope, just the definition.

So the ideal here would be to find the Definition for the function (scopes know the Definition that creates them, so this shouldn't be hard to get from the body scope) and then use `definition_expression_type` to infer types for that definition and pull the specific inferred expression types out. (Although it seems like we store on the `OverloadLiteral` for the function what known decorators it has, so if we get the type for the function definition we can maybe just use that and not need to pull out specific decorator expressions at all?

---

_@AlexWaygood reviewed on 2025-06-12 10:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6150 on 2025-06-12 10:56_

```suggestion
        } else if !current_method_decorators.contains(FunctionDecorators::STATICMETHOD) {
```

---

_Comment by @MatthewMckee4 on 2025-06-12 10:58_

i've not yet used `definition_expression_type`, but there's also tests failing and i can't see what regression ive made

---

_Comment by @codspeed-hq[bot] on 2025-06-12 11:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/MatthewMckee4%3Aincorrect-subdiagnostic-for-unresolved-reference?runnerMode=Instrumentation)

### Merging #18487 will **not alter performance**

<sub>Comparing <code>MatthewMckee4:incorrect-subdiagnostic-for-unresolved-reference</code> (ba648f9) with <code>main</code> (57bd7d0)</sub>



### Summary

`✅ 39` untouched benchmarks  





---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-06-14 12:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-14 13:07_

---

_Comment by @AlexWaygood on 2025-06-17 19:28_

There's a surprisingly large mypy_primer diff on this PR right now

---

_Comment by @carljm on 2025-06-17 20:01_

Yeah, the mypy primer diff definitely seems to indicate something unexpected is happening here. Don't have time to investigate it further right now, will have to come back to this later.

---

_Converted to draft by @sharkdp on 2025-06-27 07:52_

---

_Comment by @sharkdp on 2025-06-27 07:52_

Moving this to draft for now, feel free to move it back to review whenever you think it's ready for another round of review.

---

_@AlexWaygood approved on 2025-06-27 11:59_

Thanks, this is great. It looks like the primer hits have all gone now that https://github.com/astral-sh/ruff/pull/18809 has landed on `main`, so I think this is good to go now!

---

_Marked ready for review by @AlexWaygood on 2025-06-27 12:00_

---

_Label `bug` added by @AlexWaygood on 2025-06-27 12:38_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-27 12:38_

---

_Merged by @AlexWaygood on 2025-06-27 12:40_

---

_Closed by @AlexWaygood on 2025-06-27 12:40_

---

_Branch deleted on 2025-06-28 23:12_

---

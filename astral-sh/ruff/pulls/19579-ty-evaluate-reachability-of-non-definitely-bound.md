```yaml
number: 19579
title: "[ty] Evaluate reachability of non-definitely-bound to Ambiguous"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: reachability-ambiguous-not-definitely-bound
created_at: 2025-07-27T21:06:19Z
updated_at: 2025-10-22T15:47:23Z
url: https://github.com/astral-sh/ruff/pull/19579
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Evaluate reachability of non-definitely-bound to Ambiguous

---

_@Glyphack_

## Summary

closes https://github.com/astral-sh/ty/issues/692

If the expression (or any child expressions) is not definitely bound the reachability constraint evaluation is determined as ambiguous.

This fixes the infinite cycles panic in the following code:

```py
from typing import Literal

class Toggle:
    def __init__(self: "Toggle"):
        if not self.x:
            self.x: Literal[True] = True
```

Credit of this solution is for David.

## Test Plan

- Added a test case with too many cycle iterations panic.
- Previous tests.

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-07-27 21:09_

---

_Renamed from "Evaluate reachability of non-definitely-bound to Ambiguous" to "[ty] Evaluate reachability of non-definitely-bound to Ambiguous" by @AlexWaygood on 2025-07-27 21:09_

---

_Comment by @github-actions[bot] on 2025-07-27 21:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_util.py:255:12: error[unresolved-attribute] Type `ModuleType` has no attribute `some_func`
+ src/trio/_tests/test_util.py:256:12: error[unresolved-attribute] Type `ModuleType` has no attribute `some_func`
+ src/trio/_tests/test_util.py:258:12: error[unresolved-attribute] Type `ModuleType` has no attribute `not_funclike`
+ src/trio/_tests/test_util.py:259:12: error[unresolved-attribute] Type `ModuleType` has no attribute `_private`
+ src/trio/_tests/test_util.py:260:12: error[unresolved-attribute] Type `ModuleType` has no attribute `_private`
+ src/trio/_tests/test_util.py:261:12: error[unresolved-attribute] Type `ModuleType` has no attribute `_private`
+ src/trio/_tests/test_util.py:263:12: error[unresolved-attribute] Type `ModuleType` has no attribute `only_has_name`
+ src/trio/_tests/test_util.py:264:12: error[unresolved-attribute] Type `ModuleType` has no attribute `only_has_name`
+ src/trio/_tests/test_util.py:265:24: error[unresolved-attribute] Type `ModuleType` has no attribute `only_has_name`
+ src/trio/_tests/test_util.py:267:12: error[unresolved-attribute] Type `ModuleType` has no attribute `SomeClass`
+ src/trio/_tests/test_util.py:267:12: error[unresolved-attribute] Type `type` has no attribute `method`
+ src/trio/_tests/test_util.py:268:12: error[unresolved-attribute] Type `ModuleType` has no attribute `SomeClass`
+ src/trio/_tests/test_util.py:268:12: error[unresolved-attribute] Type `type` has no attribute `method`
+ src/trio/_tests/test_util.py:269:12: error[unresolved-attribute] Type `ModuleType` has no attribute `SomeClass`
+ src/trio/_tests/test_util.py:269:12: error[unresolved-attribute] Type `type` has no attribute `method`
+ src/trio/_tests/test_util.py:271:5: error[unresolved-attribute] Type `ModuleType` has no attribute `some_func`
+ src/trio/_tests/test_util.py:272:5: error[unresolved-attribute] Type `ModuleType` has no attribute `some_func`
+ src/trio/_tests/test_util.py:273:5: error[unresolved-attribute] Type `ModuleType` has no attribute `_private`
+ src/trio/_tests/test_util.py:274:5: error[unresolved-attribute] Type `ModuleType` has no attribute `SomeClass`
- Found 656 diagnostics
+ Found 675 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/util.py:1386:32: warning[possibly-unresolved-reference] Name `rows` used when possibly not defined
- Found 1788 diagnostics
+ Found 1789 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:65:12: warning[possibly-unbound-attribute] Attribute `vulnerability_budget` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:66:12: warning[possibly-unbound-attribute] Attribute `vulnerabilities_request_limit` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:80:12: warning[possibly-unbound-attribute] Attribute `is_first_vulnerability` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:81:12: warning[possibly-unbound-attribute] Attribute `vulnerability_budget` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:82:12: warning[possibly-unbound-attribute] Attribute `vulnerabilities_request_limit` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:83:12: warning[possibly-unbound-attribute] Attribute `vulnerabilities_request_limit` on type `IASTEnvironment | None` is possibly unbound
+ tests/appsec/iast/taint_sinks/test_vulnerability_detection.py:84:12: warning[possibly-unbound-attribute] Attribute `vulnerabilities_request_limit` on type `IASTEnvironment | None` is possibly unbound
- Found 6595 diagnostics
+ Found 6602 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/polys/numberfields/galoisgroups.py:126:21: warning[possibly-unresolved-reference] Name `current_degree` used when possibly not defined
- Found 13401 diagnostics
+ Found 13402 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/optimize/tests/test_bracket.py:318:9: error[unresolved-attribute] Unresolved attribute `count` on type `def f(x) -> Unknown`.
- Found 6420 diagnostics
+ Found 6421 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-27 21:19_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/Glyphack%3Areachability-ambiguous-not-definitely-bound?runnerMode=WallTime)

### Merging #19579 will **not alter performance**

<sub>Comparing <code>Glyphack:reachability-ambiguous-not-definitely-bound</code> (c4ce73a) with <code>main</code> (18eaa65)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-07-27 21:19_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/Glyphack%3Areachability-ambiguous-not-definitely-bound?runnerMode=Instrumentation)

### Merging #19579 will **not alter performance**

<sub>Comparing <code>Glyphack:reachability-ambiguous-not-definitely-bound</code> (c4ce73a) with <code>main</code> (18eaa65)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_Comment by @sharkdp on 2025-07-29 08:27_

@Glyphack This is in draft and tests are still failing, but please let me know if I should take a look / give initial feedback.

---

_Comment by @Glyphack on 2025-07-29 15:18_

@sharkdp please take a look. I appreciate some guidance. I left some notes in the code.
Just keep in mind the code does not work. I was trying out two ways of passing the is definitely bound information to `evaluate`:

- Record the boundness from any calls to `infer_place_load`
- Have a visitor to check boundness of the expression

---

_Comment by @github-actions[bot] on 2025-07-29 15:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @Glyphack on 2025-08-18 22:48_

David pointed out that the way I was checking if an expression is fully bound was not correct for attributes because I was not checking for the result of fallback mechanism.
Here: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/infer.rs#L7082

I added the check for result of fallback mechanism. Now the issue is that fallback mechanism is actually not reporting unbound for `self.x` in the description. It reports bound:

Result of the fallback mechanism:
```
infer_attribute_load called on attribute x
value_type.member: PlaceAndQualifiers {
    place: Type(
        BooleanLiteral(
            true,
        ),
        Bound,
    ),
    qualifiers: TypeQualifiers(
        0x0,
    ),
}
infer_attribute_load called on attribute x
value_type.member: PlaceAndQualifiers {
    place: Unbound,
    qualifiers: TypeQualifiers(
        0x0,
    ),
}
```

Now I'm checking the fallback and `own_instance_member` method to see how we end up with a Bound place here. I don't think we should say this value is bound. In the example the attribute is not bound.

---

_@sharkdp reviewed on 2025-08-20 15:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2532 on 2025-08-20 15:49_

The lines here only handle *declared* instance attributes (like `self.x: Literal[True] = True` in the `Toggle` example). We should do the same for non-declared instance attributes (e.g. `self.x = True`). This modified `Toggle` example shows that this is currently still problematic on this branch:
```py
from typing import Literal

def check(x) -> Literal[False]:
    return False

class Toggle:
    def __init__(self: "Toggle"):
        if check(self.x):
            self.x = True
```

I had to use a little trick here: `if not self.x` wouldn't work anymore when `self.x` is not declared. For undeclared symbols, we infer `Unknown | True`, and the `Unknown` will not lead to a definitive Boolean answer. So I added this `check(…)` function that always returns `Literal[False]`.

---

_Marked ready for review by @Glyphack on 2025-08-20 19:36_

---

_Review requested from @carljm by @Glyphack on 2025-08-20 19:36_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-08-20 19:36_

---

_Review requested from @dcreager by @Glyphack on 2025-08-20 19:36_

---

_@Glyphack reviewed on 2025-08-20 19:36_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/types/class.rs`:2532 on 2025-08-20 19:36_

Thanks, addressed it here:
https://github.com/astral-sh/ruff/pull/19579/commits/f8626ce9ae7c74e86d76c8a55e17c921a0a1380e


---

_Comment by @Glyphack on 2025-08-20 19:49_

I feel this is ready for review, beside this part that I changed to make the idea work:
https://github.com/astral-sh/ruff/pull/19579/files#diff-8049ab5af787dba29daa389bbe2b691560c15461ef536f122b1beab112a4b48aR6585

The challenge there was that we are getting a possibly bound place back in the lookup in implicit_attribute_inner, and return a `Place::Bound`. This is okay for implicit attributes. But in this PR I wanted to know if something is definitely bound or not. Hence I added this.

Other things came to mind was return more information from this function to indicate this. I was not sure what is the correct way to model this.

---

_@sharkdp reviewed on 2025-08-22 15:35_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6585 on 2025-08-22 15:35_

Thank you for pushing this PR further forward! Now that we know that this approach works in general, I think we should invest a bit of time to see if there are less-hacky solutions here. I'm happy to do that part myself, but if you have additional ideas, or want to keep working on this, please let me know.

Tracking `NOT_BOUND` as a `TypeQualifier` really *does* feel like a hack. If I understand this correctly, the problem is the following: we do want to keep track of the fact that we found an implicit instance attribute binding/declaration that is possibly unbound (according to control-flow analysis inside the body of the class method it is defined in; *all* instance attributes that are defined in non-`__init__` methods are sort of possibly-unbound anyway, because we don't know if a method has been called on an object or not). Since it is so common that implicit instance attributes have this state, we do not want to emit diagnostics for each of these possibly-unbound attributes.

We can't simply report `Boundness::PossiblyUnbound` for these implicit instance attributes, because we mix them with other attributes like class-level attributes higher up in the call chain (for which we *do* want to track boundness). So when type inference code accesses `.member` on a given type, we wouldn't know whether it's a possibly-unbound class attribute or a possibly-unbound implicit instance attribute.

What's especially weird about tracking this as `TypeQualifier::NOT_BOUND` is that this contradicts the `Boundness` information. I wonder if we could return the actual boundness state in the `Boundness` field, and only add the additional information about the source of this attribute in the return type of `.member` and related methods. So instead of returning `PlaceAndQualifiers`, we would maybe return a `Member` struct that additionally contains some sort of `AttributeSource { ClassBody, ImplicitInstanceAttribute }` enum (which might also become valuable for other use cases, I think). We could then ignore `AttributeSource::ImplicitInstanceAttribute` for purposes of emitting diagnostics, but still use the proper boundness information for purposes of evaluating reachability constraints.

---

_@sharkdp reviewed on 2025-08-22 15:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:344 on 2025-08-22 15:36_

Can we remove this, or make it more concise to mention only what's relevant for posterity?

---

_Comment by @Glyphack on 2025-08-22 15:49_

Thank you for the feedback. I'm drafting the PR to apply the suggestion.

---

_Converted to draft by @Glyphack on 2025-08-22 15:50_

---

_Comment by @sharkdp on 2025-08-27 13:11_

I'm looking into making the changes described above.

---

_@sharkdp reviewed on 2025-08-27 14:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:8728 on 2025-08-27 14:23_

Yeah, this fails because this first `infer_place_load` returns `Unbound`. We would have to check the fallback solutions as well. I don't know how a subscript expression could return `PossiblyUnbound`, so I think it's fine to just remove this.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-27 15:36_

---

_Comment by @github-actions[bot] on 2025-08-27 15:45_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 20 | 0 | 0 |
| `possibly-unbound-attribute` | 7 | 0 | 0 |
| `possibly-unresolved-reference` | 2 | 0 | 0 |
| **Total** | **29** | **0** | **0** |


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-28 07:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-28 07:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-28 11:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-28 11:51_

---

_Comment by @sharkdp on 2025-08-28 11:59_

I tried various other solutions here, and non of them worked as well as what @Glyphack implemented in the first place. The main problem is that we can only build unions/intersections of types. We can't build unions/intersections of `TypeQualifier`s and boundness information, which requires us to merge them somehow when we operate on unions/intersections (for example, when we access an attribute on a union/intersection). With this representation, it is impossible to distinguish between a case where we have `C | None` (where an attribute access should definitely yield possibly-unbound, due to the `None` in the union), and something like `C | Unknown` (where we would like to silence the possibly-unbound-attribute diagnostic if the attribute is only possibly bound inside a method of `C`).

I also tried various completely different approaches of solving this problem, and none of them hit the right granularity. Either they are not able to solve the `Toggle` problem, or they are too big of a hammer, and lead to insufficient reachability analysis in real world use cases.

So I am returning this PR to a previous state (with a couple of smaller cleanups, renamings, and additional tests). I am still not very satisfied with the fact that we track boundness information in a `TypeQualifier`, but I'm out of ideas. And we would very much like to proceed with type-of-`self`.

---

_Marked ready for review by @sharkdp on 2025-08-28 12:02_

---

_@sharkdp approved on 2025-08-28 12:03_

Thank you again, @Glyphack!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/statically_known_branches.md`:1570 on 2025-08-28 12:08_

```suggestion
truthiness. If we didn't do that, `x` would be considered definitely bound in the following
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:139 on 2025-08-28 12:08_

```suggestion
    pub(crate) const fn is_definitely_bound(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8637 on 2025-08-28 12:09_

Can this not be `#[derive()]`d?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:342 on 2025-08-28 12:10_

```suggestion
/// Returns [`Truthiness::Ambiguous`] in case any non-definitely bound places
```

---

_@AlexWaygood approved on 2025-08-28 12:13_

Thank you!

---

_Merged by @sharkdp on 2025-08-28 12:34_

---

_Closed by @sharkdp on 2025-08-28 12:34_

---

_Branch deleted on 2025-10-22 15:47_

---

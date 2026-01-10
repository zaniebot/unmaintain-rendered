```yaml
number: 13164
title: "[red-knot] implement basic call expression inference"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-call-expression-inference
created_at: 2024-08-30T15:31:56Z
updated_at: 2024-09-05T15:27:24Z
url: https://github.com/astral-sh/ruff/pull/13164
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] implement basic call expression inference

---

_Pull request opened by @chriskrycho on 2024-08-30 15:31_

## Summary

Adds basic support for inferring the type resulting from a call expression. This only works for the *result* of call expressions; it performs no inference on parameters. It also intentionally does nothing with class instantiation, `__call__` implementors, or lambdas.

## Test Plan

Adds a test that it infers the right thing!


---

_Review requested from @carljm by @chriskrycho on 2024-08-30 15:31_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-30 15:31_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-30 15:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1752 on 2024-08-30 15:37_

We shouldn't be matching on the syntactic form of `func` here; the inner match on the type of `func` should really be the only one here. And we don't need to go digging into `self.types.expressions` for that type. `self.infer_expression(func)` already returns the inferred type for `func` (we just aren't using it), and then we can just match on that type. It shouldn't matter to us here whether `func` is a simple `Name`, or an attribute expression, or whatever else, those details are all handled by `infer_expression`; all that matters here is the type we infer for it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1764 on 2024-08-30 15:38_

This second TODO shouldn't be associated with `Type::Class`, rather it should go with an arm for `Type::Instance`, which is the type representing an instance of a class (as opposed to the class object itself, which `Type::Class` represents.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1758 on 2024-08-30 15:40_

I think we will want to follow the existing pattern of `Type::member` here, with something like `Type::call`, so this match on `Type` variants happens inside `Type` rather than in type inference (the code here would reduce to basically a one liner like `func_ty.call()`). The main reason for this is that the definition of `call` for unions and intersections (which doesn't have to be implemented in this PR) will involve recursively calling `call` on the elements of the union/intersection.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1772 on 2024-08-30 15:43_

We shouldn't use `todo!()` in cases we aren't handling yet, because this will result in panics analyzing some code, which we want to avoid. (I think we are in fact seeing this in the benchmark failure here.) The pattern to use instead for things we don't handle yet is `Type::Unknown` plus a `TODO` comment.

---

_Comment by @github-actions[bot] on 2024-08-30 15:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1774 on 2024-08-30 15:46_

There aren't a ton of examples of this in the code yet, because we only recently gained the ability to emit diagnostics -- but we do have that ability now, and I think we should start actually emitting them where it is correct to do so. So here we should emit a diagnostic with the message `{type} is not callable`. (We could also add a TODO for that instead if you find it adds a lot of complexity for some reason, but I don't think it should?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2835 on 2024-08-30 15:47_

Fantastic, this is exciting to see happen! Unlocks so much new capability.

---

_@carljm requested changes on 2024-08-30 15:48_

Looks good! Some things we should adjust in the implementation.

---

_@chriskrycho reviewed on 2024-08-30 15:51_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1772 on 2024-08-30 15:51_

Doh! I thought I’d updated all of those. Thank you. 👍🏼

---

_@chriskrycho reviewed on 2024-08-30 15:52_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1774 on 2024-08-30 15:52_

Ah, interesting – from previous discussions with @AlexWaygood I was under the (mistaken! And I don’t blame Alex for this) impression that we deferred *all* handling of diagnostics to later in the pipeline. Will update. 👍🏼

---

_@carljm reviewed on 2024-08-30 15:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1768 on 2024-08-30 15:53_

Oh, I think we should also add an arm for `Type::Unknown => Type::Unknown`; that'll be important to avoid tons of noise in the short term from the diagnostic I suggested adding below.

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1752 on 2024-08-30 15:54_

Ah, excellent. 💯 

---

_@chriskrycho reviewed on 2024-08-30 15:54_

---

_@chriskrycho reviewed on 2024-08-30 15:57_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1758 on 2024-08-30 15:57_

Ahhhh, that makes sense. 👍🏼 Will move this over there!

---

_@AlexWaygood reviewed on 2024-08-30 15:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1774 on 2024-08-30 15:58_

I think it's not a priority for us to emit comprehensive diagnosis for all typing errors right now, but I agree with @carljm that it makes sense to start emitting diagnostics where it makes sense to do so 👍 it helps us see how things all fit together into a bigger picture

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:327 on 2024-08-30 17:51_

Let's add a doc comment to this function, something like this:
```suggestion
    /// Return the type resulting from calling an object of this type.
    ///
    /// Returns `None` if `self` is not a callable type.
    #[must_use]
    pub fn call(&self, db: &'db dyn Db) -> Option<Type<'db>> {
```

---

_@carljm approved on 2024-08-30 17:53_

Awesome, thank you!!

---

_Comment by @carljm on 2024-08-30 18:01_

Looks like we need to adjust the expected diagnostics in the benchmark.

---

_Comment by @codspeed-hq[bot] on 2024-08-30 18:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/chriskrycho:rk-call-expression-inference)

### Merging #13164 will **degrade performances by 14.73%**

<sub>Comparing <code>chriskrycho:rk-call-expression-inference</code> (4c52a81) with <code>main</code> (a73bebc)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `chriskrycho:rk-call-expression-inference` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[incremental]` | 2.4 ms | 2.8 ms | -14.73% |


---

_Comment by @carljm on 2024-08-30 19:02_

Looks like this CodSpeed regression is real, but it's expected; we are now looking up the return type of every function called (and apparently doing it twice.) Fixing the double inference part should reduce the size of the regression somewhat.

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:27 on 2024-08-30 19:03_

This feels like not-a-great error message for symbols with type `Unbound` specifically... but we can probably hone that later. (But maybe we should add a TODO somewhere?)

---

_@AlexWaygood reviewed on 2024-08-30 19:03_

---

_@carljm reviewed on 2024-08-30 19:07_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:27 on 2024-08-30 19:07_

I feel like until we are actually making a concerted effort to polish up our diagnostics across red-knot, it's a little hard to know how we'd actually want to implement an improvement here, and thus where to even put the TODO. So I'm inclined to just leave this alone for now.

---

_@carljm reviewed on 2024-08-30 19:14_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:27 on 2024-08-30 19:14_

(Especially considering I'm not 100% convinced that we'll even keep `Type::Unbound` around.)

---

_@carljm reviewed on 2024-08-30 19:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:599 on 2024-08-30 19:16_

```suggestion
```

---

_Comment by @carljm on 2024-08-30 19:26_

On closer look, I think a lot of the regression (and why it shows up only in incremental benchmark) is that annotations are deferred in stubs, and now we look up a lot of return types from stubs, which causes us to trigger the `infer_deferred_types` query on a lot more function definitions, creating more memoized query results that Salsa has to re-validate in an incremental re-check. I still think this is expected and there's not a lot we can do about it, short of finding ways to make Salsa more efficient on new revisions when there are a lot of ingredients to re-validate.

---

_@AlexWaygood reviewed on 2024-08-30 19:36_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:27 on 2024-08-30 19:36_

Fair enough! In general, usability is quite high priority for me, and I worry that this edge case might be harder to spot once our analysis improves (since I'm guessing the symbol in question here shouldn't have `Type::Unbound` to begin with). But I see your point.

---

_Merged by @carljm on 2024-08-30 19:51_

---

_Closed by @carljm on 2024-08-30 19:51_

---

_Branch deleted on 2024-08-30 21:15_

---

_Label `red-knot` added by @dhruvmanila on 2024-09-05 15:27_

---

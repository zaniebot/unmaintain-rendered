```yaml
number: 13711
title: "[red-knot] Do not panic if named expressions show up in assignment position"
type: pull_request
state: merged
author: rtpg
labels:
  - ty
assignees: []
merged: true
base: main
head: assignment
created_at: 2024-10-11T06:38:50Z
updated_at: 2024-10-16T12:51:49Z
url: https://github.com/astral-sh/ruff/pull/13711
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Do not panic if named expressions show up in assignment position

---

_Pull request opened by @rtpg on 2024-10-11 06:38_

Unfortunately the test I added here still fails (I believe due to scoping issues, still trying to wrap my head around how the scoping is supposed to work), but I believe my changes to the builder are correct: it is not incorrect for there to be `current_assignment`s to juggle, given the existence of named expressions.

---

_Label `red-knot` added by @carljm on 2024-10-11 19:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:566 on 2024-10-11 19:14_

I think it's still correct to assert that the length of `self.current_assignments` is 0 here? A NamedExpr should be the only kind of assignment that can be nested inside the target of another assignment.

Same for all other cases, except for `NamedExpr`, where this assertion was removed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3444 on 2024-10-11 19:41_

This is invalid syntax. Red-knot isn't resilient to arbitrary invalid syntax yet (though we should become so). We should make a change so that for now red-knot just always fails fast if there are parser errors, rather than hitting panics that are hard to debug and cause confusion; this isn't the first time we've had this scenario.

For this to be valid syntax, the namedexpr has to be parenthesized to fix the binding precedence. If we do this, the test passes:
```suggestion
            x[0 if (y := 2) else 1] = 1
```

---

_@carljm requested changes on 2024-10-11 19:42_

Thank you!! I had totally failed to consider the possibility of a namedexpr in a LHS subscript expression.

---

_@carljm reviewed on 2024-10-11 19:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3444 on 2024-10-11 19:59_

The new mdtest framework will already help with this, since it won't hide syntax errors like the current tests do. Currently it will just fail fast on any test with a syntax error in it; once we start working on syntax error resilience we will add features to it to explicitly assert on syntax errors, but tests with accidental syntax errors will still fail with a clear diagnostic.

---

_@rtpg reviewed on 2024-10-12 01:52_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:3444 on 2024-10-12 01:52_

Ah, great catch, will fix the test. Glad to see these changes are able to be handled.

 My current reading is that the semantic index expects everything to be covered so even in the case of syntax errors we would be expected to try and see each symbol anyways... but that might be me buying into the error's premise a bit too much.

Like should the semantic index cover the entire program? If so, it feels like the underlying error is still a problem. If not, then it feels like there's some concept here of not being covered (and some of the philosophy of #13701, of having lookups be `Option`/`Result`-y and falling back, could apply).


---

_@rtpg reviewed on 2024-10-12 02:03_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:566 on 2024-10-12 02:03_

That's a good point, since you don't have statements in statements. Re-evaluated the changeset and agree with the assertion that we can keep all the assertions.

---

_@carljm reviewed on 2024-10-12 02:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3444 on 2024-10-12 02:36_

The panic here with the syntax error isn't exactly due to lack of coverage. It's that we currently assume (reasonably!) that if a name is being defined, there will be a Name node with Store context; that's normally how the AST works. [With the malformed syntax](https://play.ruff.rs/566caf2d-66f7-443c-80a3-fc189ab29751), we end up with a Named expression node whose target doesn't contain any Name node with Store context. So we never create any `Definition` for the `Named`. But then type inference assumes that every Named node should be associated with a `Definition`, it tries to look up the `Definition` and there isn't one.

I agree with you in general that becoming syntax-error-resilient will probably mean making fewer such invariant assumptions, and being more willing to fall back to Unknown when something just doesn't make sense. The loss with doing this unconditionally is that it becomes easier for bugs to go unnoticed and propagate silently. Ideally perhaps we could relax our invariants only when we actually observe invalid nodes in the AST, rather than doing so unconditionally, to preserve a bit more fail-fast when we have bugs. I'm not sure if this will be feasible or not. The truth is that we have a lot to do and syntax-error-resilience hasn't made it to the top of the list yet!

I think this particular panic would be fixed by having `infer_named_expr` in type inference check if the target of the Named node is a Name with Store context (which it always should be, if syntax is valid -- walrus expressions aren't allowed to have complex left-hand side), and if not, bail out and don't try to look up a Definition at all. But I would kind of like to consider our strategy for syntax-error-resilience a bit more holistically.

cc @dhruvmanila who is intending to take on the syntax-error-resilience at some point.

I hadn't seen https://github.com/astral-sh/ruff/pull/13701 yet. (Note, I generally won't be alerted to the existence of draft PRs unless explicitly pointed to them; they don't show up in reviewer notifications until marked ready for review.) I'll take a look at it soon (may not be til Monday.)

---

_Marked ready for review by @rtpg on 2024-10-14 06:01_

---

_Review requested from @MichaReiser by @rtpg on 2024-10-14 06:01_

---

_Review requested from @AlexWaygood by @rtpg on 2024-10-14 06:01_

---

_Comment by @rtpg on 2024-10-14 06:02_

I am a bit mystified by the benchmark/ecosystem failures but I think the code changes here are ready for another review.

---

_Comment by @MichaReiser on 2024-10-14 14:08_

The benchmark fail at 

https://github.com/astral-sh/ruff/blob/47a3625f79313971014473664e023514f13816b7/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L420

Indicating that there are assignments that weren't popped correctly or that the assertion needs updating with your changes.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:368 on 2024-10-14 14:09_

You want to move this out of a debug assertion or the `pop` call only happens during debugging. It probably makes sense to move this into a helper function. 

```suggestion
				let assignment = self.current_assignments.pop();
				debug_assert_ne!(assignment, None);
```

---

_@MichaReiser reviewed on 2024-10-14 14:09_

---

_@rtpg reviewed on 2024-10-14 22:37_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:368 on 2024-10-14 22:37_

right, of course. Thanks for pointing that out.

---

_@rtpg reviewed on 2024-10-15 00:10_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:368 on 2024-10-15 00:10_

Went with adding `push_assignment` and `pop_assignment` as a pair of functions for this operation.

I thought about `with_assignment` a bit but I am unsure of the relative cost of closure building here. Given the final assert at the end of building being in place, might not be much of an issue in practice.

---

_Comment by @github-actions[bot] on 2024-10-15 00:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:432 on 2024-10-15 09:22_

Nit: i like to do 
```suggestion
        assert_eq!(&self.current_assignments, &[]);
```

it gives better error messages if the vec isn't empty by printing all assignments.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3671 on 2024-10-15 09:23_

We should avoid adding new tests to `infer.rs` and instead use our type inference testing framework. https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test

---

_@MichaReiser reviewed on 2024-10-15 09:23_

---

_Review requested from @carljm by @MichaReiser on 2024-10-15 09:23_

---

_@rtpg reviewed on 2024-10-16 00:29_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:432 on 2024-10-16 00:29_

OK, changed to that (along with deriving `PartialEq` on `CurrentAssignment` to get that to work, turns out we hadn't been doing any equality checks on those before this change)

---

_@rtpg reviewed on 2024-10-16 00:32_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/resources/mdtest/literal/collections/list.md`:40 on 2024-10-16 00:32_

I just realized that this was in `literal/collections/list.md` and not `collections/list.md`. Please advise on whether this should be extracted to be placed elsewhere

---

_@rtpg reviewed on 2024-10-16 01:55_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:3671 on 2024-10-16 01:55_

Moved this over to an md test

---

_@carljm reviewed on 2024-10-16 12:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/collections/list.md`:40 on 2024-10-16 12:30_

I would put these tests in `subscript/list.md`.

---

_@carljm approved on 2024-10-16 12:32_

Looks great, thank you! I will go ahead and move the tests and then merge.

---

_Merged by @carljm on 2024-10-16 12:42_

---

_Closed by @carljm on 2024-10-16 12:42_

---

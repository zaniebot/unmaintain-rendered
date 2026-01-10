```yaml
number: 13943
title: "[red-knot] Infer `tuple` types from annotations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/infer-tuples
created_at: 2024-10-27T21:56:48Z
updated_at: 2025-05-07T15:23:05Z
url: https://github.com/astral-sh/ruff/pull/13943
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Infer `tuple` types from annotations

---

_Pull request opened by @AlexWaygood on 2024-10-27 21:56_

## Summary

This PR adds support for heterogenous `tuple` annotations to red-knot.

I initially thought that this might be useful for `sys.version_info` support... now that I look again, I'm not sure that it _is_ actually useful for that. Anyway, still a useful feature, and one we'd have had to implement anyway!

The PR does the following:
- Extends `infer_type_expression` so that it understands tuple annotations
- Changes `infer_type_expression` so that `ExprStarred` nodes in type annotations are inferred as `Todo` rather than `Unknown` (they're valid in PEP-646 tuple annotations)
- Extends `Type::is_subtype_of` to understand when one heterogenous tuple type can be understood to be a subtype of another (without this change, the PR would have introduced new false-positive errors to some existing mdtests).

## Test Plan

- mdtests added to `assignment/annotations.md`
- `is_subtype_of` unit tests have been added to `types.rs`
- I verified that the PR produces no new false positives in the red-knot benchmarks.


---

_Label `red-knot` added by @AlexWaygood on 2024-10-27 21:56_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-27 21:56_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-27 21:56_

---

_Comment by @AlexWaygood on 2024-10-27 22:50_

No idea what's up with those CI errors; they don't really seem to be anything to do with this PR

---

_Comment by @MichaReiser on 2024-10-28 07:21_

> No idea what's up with those CI errors; they don't really seem to be anything to do with this PR

This should be fixed now. Cargo insta made the `--unreferenced-snapshots` detection more correct and that caught a few unreferenced snapshot in our repository. @dhruvmanila deleted them on main, so you should be good to go 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:478 on 2024-10-28 07:28_

Nit: Not sure if Rustc can deduplicate the `self_tuple.elements(db).len()` (inlined `len`) and the `self_tuple(db)` because of the dyn `Db`. 

```suggestion
								let self_elements = self_tuple.elements(db);
								let target_elements = target_tuple.elements(db);
								
                self_elements.len(db) == target_elements.len(db)
                    && self_elements
                        .iter()
                        .zip(target_elements)
                        .all(|(self_element, target_element)| {
                            self_element.is_subtype_of(db, *target_element)
                        })
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3510 on 2024-10-28 07:29_

```suggestion
                            let mut elements = Vec::with_capacity(tuple.elements.len());
```

or use collect?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3514 on 2024-10-28 07:32_

I thought we only use `Todo` if the inference itself is directly the consequence of something we aren't handling but not when we continue deriving other types from it... which seems to be the case here?

I think I would prefer a type `(str, Todo, int)` over just `Todo` because this gives me less information where the `Todo` is coming from and makes it harder to track down

---

_@MichaReiser reviewed on 2024-10-28 07:32_

---

_@AlexWaygood reviewed on 2024-10-28 07:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3514 on 2024-10-28 07:45_

The issue here is that certain parameters can change the meaning of the entire tuple. If we only inferred Todo for a subelement of a tuple here, we'd interpret the annotation `tuple[str, ...]` as `tuple[str, Todo]` which means "a tuple of length two where the first element is a `str` and the second element is todo". But that's flat-out wrong — `tuple[str, ...]` actually means "a tuple of arbitrary length, in which all elements in the tuple are instances of `str`" — the `...` changes the meaning of the entire tuple (it becomes a homogenous tuple rather than a heterogeneous tuple, and we [don't support homogenous tuples yet](https://github.com/astral-sh/ruff/issues/13855)).

Hmmm... But maybe I can limit the situations where we return Todo for the entire tuple to certain special elements that we know change the meaning of the entire tuple... And just infer Todo for subelements rather than the whole tuple otherwise.

---

_@AlexWaygood reviewed on 2024-10-28 10:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3514 on 2024-10-28 10:34_

I made this change. It didn't really make _much_ impact, because even `Unpack[tuple[int, ...]]` as a subelement can change the meaning of the entire tuple, so we still need to infer `Todo` for the entire tuple if an `Expr::Subscript` is found in the tuple that we infer to be `Todo`. But, there are now a few more tuples where we only infer `Todo` for a subelement rather than the whole tuple. And, I made the comments more expansive to explain why we need to do it this way :-)

---

_Comment by @github-actions[bot] on 2024-10-28 10:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:63 on 2024-10-28 21:32_

nit: when I saw this header, I expected to find "incorrect tuple annotations", as in invalid tuple annotation forms. What I found instead I would describe as "invalid tuple assignments"
```suggestion
## Incorrect tuple assignments are complained about
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:21 on 2024-10-28 21:33_

If we're editing this message anyway, let's use language from the Python typing spec instead of Hindley-Milner language
```suggestion
# TODO should emit a diagnostic here (str is not assignable to int)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:472 on 2024-10-28 21:37_

This raises interesting questions about how we handle union-of-tuples vs tuple-of-unions, as in, the equivalence of `tuple[int, str | int]` to `tuple[int, str] | tuple[int, int]`. I'm not sure how important it is that we understand this (I don't think existing type checkers do, but it would be cool if we did.)

We could try to normalize this one way or another in type construction, at the cost of less fidelity to original annotation forms. Or we could try to handle it here, at potentially a high complexity cost. Or we could decide not to handle it at all.


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3766 on 2024-10-28 21:40_

A sizable percentage of this entire function ends up being Todo-handling. All else equal, I'd rather not write tons of extra code for accurate handling of Todos, since medium-term they should disappear. But on the other hand, I think this still falls on the "probably worth it for debugging value" side of the line.

---

_@carljm approved on 2024-10-28 21:41_

Looks great, thank you!!

---

_@carljm reviewed on 2024-10-28 22:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:472 on 2024-10-28 22:57_

(To be clear, I don't think we need to do any of this in this PR.)

---

_@AlexWaygood reviewed on 2024-10-29 10:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:472 on 2024-10-29 10:22_

Yeah, this is an interesting question... I'd be curious to see examples of exactly which things we _should_ infer (and _would_ infer if we understood this expansion), but don't currently

---

_Merged by @AlexWaygood on 2024-10-29 10:30_

---

_Closed by @AlexWaygood on 2024-10-29 10:30_

---

_Branch deleted on 2024-10-29 10:30_

---

_@carljm reviewed on 2024-10-29 11:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:472 on 2024-10-29 11:45_

I think it's less about inference and more about simple assignability. If a function parameter is annotated as the one type and you pass it an argument that ends up getting inferred as the other, that call should be accepted but existing type checkers won't accept it. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:472 on 2024-10-29 11:46_

Ah, makes sense

---

_@AlexWaygood reviewed on 2024-10-29 11:46_

---

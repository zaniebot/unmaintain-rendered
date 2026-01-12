```yaml
number: 14151
title: "[red-knot] Add support for string annotations"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/string-annotation
created_at: 2024-11-07T12:06:02Z
updated_at: 2024-11-15T04:19:34Z
url: https://github.com/astral-sh/ruff/pull/14151
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Add support for string annotations

---

_@dhruvmanila_

## Summary

This PR adds support for parsing and inferring types within string annotations.

### Implementation (attempt 1)

This is preserved in https://github.com/astral-sh/ruff/pull/14151/commits/6217f48924f8f3f8a3d28c4929fe5aaad4ad0a59.

The implementation here would separate the inference of string annotations in the deferred query. This requires the following:
* Two ways of evaluating the deferred definitions - lazily and eagerly. 
	* An eager evaluation occurs right outside the definition query which in this case would be in `binding_ty` and `declaration_ty`.
	* A lazy evaluation occurs on demand like using the `definition_expression_ty` to determine the function return type and class bases.
* The above point means that when trying to get the binding type for a variable in an annotated assignment, the definition query won't include the type. So, it'll require going through the deferred query to get the type.

This has the following limitations:
* Nested string annotations, although not necessarily a useful feature, is difficult to implement unless we convert the implementation in an infinite loop
* Partial string annotations require complex layout because inferring the types for stringified and non-stringified parts of the annotation are done in separate queries. This means we need to maintain additional information

### Implementation (attempt 2)

This is the final diff in this PR.

The implementation here does the complete inference of string annotation in the same definition query by maintaining certain state while trying to infer different parts of an expression and take decisions accordingly. These are:
* Allow names that are part of a string annotation to not exists in the symbol table. For example, in `x: "Foo"`, if the "Foo" symbol is not defined then it won't exists in the symbol table even though it's being used. This is an invariant which is being allowed only for symbols in a string annotation.
* Similarly, lookup name is updated to do the same and if the symbol doesn't exists, then it's not bounded.
* Store the final type of a string annotation on the string expression itself and not for any of the sub-expressions that are created after parsing. This is because those sub-expressions won't exists in the semantic index.

Design document: https://www.notion.so/astral-sh/String-Annotations-12148797e1ca801197a9f146641e5b71?pvs=4

Closes: #13796 

## Test Plan

* Add various test cases in our markdown framework
* Run `red_knot` on LibCST (contains a lot of string annotations, specifically https://github.com/Instagram/LibCST/blob/main/libcst/matchers/_matcher_base.py), FastAPI (good amount of annotated code including `typing.Literal`) and compare against the `main` branch output


---

_Label `red-knot` added by @dhruvmanila on 2024-11-07 12:06_

---

_Comment by @github-actions[bot] on 2024-11-13 07:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "WIP: Add support for string annotations" to "[red-knot] Add support for string annotations" by @dhruvmanila on 2024-11-13 12:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:343 on 2024-11-13 18:08_

TODO: add some documentation

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:678 on 2024-11-13 18:08_

TODO: add a comment on why annotated assignment are not deferred

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4632 on 2024-11-13 18:09_

TODO: add documentation for the enum and it's variants

---

_@dhruvmanila reviewed on 2024-11-13 18:09_

---

_@dhruvmanila reviewed on 2024-11-13 18:10_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1106 on 2024-11-13 18:10_

The diff is to remove the `self.are_all_types_deferred()` check because we're already in a deferred query here.

---

_Marked ready for review by @dhruvmanila on 2024-11-13 18:11_

---

_Review requested from @carljm by @dhruvmanila on 2024-11-13 18:11_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-13 18:11_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-11-13 18:11_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-11-13 18:11_

---

_Comment by @dhruvmanila on 2024-11-13 18:11_

Although there are a couple of TODOs that I want to address mostly around documentation, I'd appreciate some initial feedback for any obvious issues that I've missed.

---

_@MichaReiser reviewed on 2024-11-13 18:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:459 on 2024-11-13 18:38_

Is it possible that inferring the deferred types add new entries to `self.types.deferred`?

I would probably write this as
```suggestion
				let deferred = std::mem::take(&mut self.deferred);
				for definition in deferred {
					self.extend(infer_derferred_types(self.db, *definition));
				}
        
        assert!(self.deferred.is_empty(), "Inferring deferred types shouldn't add new types to infer"); 
```

---

_@MichaReiser reviewed on 2024-11-13 18:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4076 on 2024-11-13 18:41_

```suggestion
        let previous_deferred_state = std::mem::replace(&mut self.deferred_state, deferred_state);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:403 on 2024-11-13 23:46_

Not sure if I'm missing an issue here, but maybe inferring an `InferenceRegion::Deferred` should set `self.deferred_state` and then this check can be simplified?

You have a note above to add docs for `deferred_state`; I think how these two things relate (and why `InferenceRegion::Deferred` overrides `self.deferred_state`) is a good candidate for some clear docs.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:3 on 2024-11-13 23:46_

These tests are fabulous! Thorough, clear, and correct, the testing trifecta :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:459 on 2024-11-13 23:49_

I agree it's important we either enforce and check that inferring deferred doesn't add more deferred, or else have explicit handling in case it does.

---

_@carljm reviewed on 2024-11-13 23:53_

Didn't complete review yet, but I have to run, so submitting the few comments I have so far in case I don't get back to it tonight.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:678 on 2024-11-14 05:17_

I'm curious to read this comment!

Could we add some tests that annotations on annotated assignments in stub files and when `from __future__ import annotations` is active, are in fact deferred? (That is, can contain forward references.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1638 on 2024-11-14 05:20_

Why the change to use `assignment.target` here instead of the `target` we unpacked above? Shouldn't they be the same?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1651 on 2024-11-14 05:20_

Why the change to use `assignment.value` here and no longer unpack `value` above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/string_annotation.rs`:60 on 2024-11-14 05:37_

```suggestion
            // case for annotations that contain escape sequences.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:403 on 2024-11-14 05:40_

Ok after reading the whole diff more carefully, I think I understand this now, but yeah it would be great to document it.

---

_@carljm approved on 2024-11-14 05:42_

This is excellent, great work! It turned out way less complicated than I thought it would be. I love that with a few small changes we can sidestep the whole issue of AST IDs and just store the resulting type on the string node. I think the result of this is that we won't be able to have hover types for individual parts of a string annotation, just the whole annotation, but personally I think that's fine; it is in fact just one expression, after all.

---

_Comment by @MichaReiser on 2024-11-14 06:44_

> I think the result of this is that we won't be able to have hover types for individual parts of a string annotation, just the whole annotation, but personally I think that's fine; it is in fact just one expression, after all.

Do we know how we would extend this design to support resolving types of inner expressions? Do we know if pyright supports hovering for stringified type annotations? Not supporting expression-level-types might be problematic long term because it doesn't just affect hovering:

* No hover support
* No go-to-definition support, e.g. I can't jump to `MyType` in `a: "str | MyType"` which is somewhat annoying
* Possibly: types in stringified type annotations won't show up in find-all-references
* Ruff's lint rule can't run on stringified type annotations. That would be a large regression feature-wise. For example, I believe it would make it more complicated to implement https://docs.astral.sh/ruff/rules/never-union/ because we can only ask for the outer type but then are stuck, because we don't know if the `Never` type comes from the annotation itself (because it uses a `Union`) or is the result of a type alias, type var, or something else.

---

_Comment by @dhruvmanila on 2024-11-14 11:33_

> Do we know if pyright supports hovering for stringified type annotations?

Yes, Pyright does support hover, goto definitions, references, etc. in string annotations.

Looking a bit deeper into Pyright, I think the reason they're able to do this is because they parse string annotations during the parsing stage and store it on the AST. But, the thing that I missed, and is not in the design document, is that the Pyright parser also takes into account `typing.Literal` and `typing.Annotated` with a limitation that it won't resolve the import and only consider it if it's exactly being imported from `typing` or `typing_extensions` (https://github.com/microsoft/pyright/blob/294b7afd2eaf23b0586bcc8563571bdff0c0d0a6/packages/pyright-internal/src/parser/parser.ts#L3610-L3621).

> Do we know how we would extend this design to support resolving types of inner expressions?

I and Micha talked about this in our 1:1 today and I think it's fine to move forward with this implementation today and visit it at a later stage. We might have to spend some additional time in figuring out the LSP part though. It might also be useful to understand the user impact of this feature if and when this needs to be implemented to validate the time investment. Some of the ideas for the implementation would be (a) updating the parser / AST to accommodate the change (b) semantic index that's specific to string annotation and an additional layer that connects the semantic index from the file with these specific ones.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:459 on 2024-11-14 11:53_

Yeah, makes sense, thanks

---

_@dhruvmanila reviewed on 2024-11-14 11:53_

---

_@dhruvmanila reviewed on 2024-11-14 12:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1638 on 2024-11-14 12:00_

Sorry, this is just a leftover from my initial attempt, will revert

---

_@dhruvmanila reviewed on 2024-11-14 12:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1651 on 2024-11-14 12:00_

Same as above

---

_@dhruvmanila reviewed on 2024-11-14 12:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:678 on 2024-11-14 12:56_

Added the test cases in `assignment/annotations.md`

---

_Comment by @carljm on 2024-11-14 15:19_

Yes, I think (b) describes how I'd envisioned we could tackle this, if/when we need to. I think it might also be possible to do something even simpler that doesn't do a full semantic index of the AST from the stringified annotation, but just adds a new mechanism for attaching a type directly to a text range instead of a node?

---

_Merged by @dhruvmanila on 2024-11-15 04:10_

---

_Closed by @dhruvmanila on 2024-11-15 04:10_

---

_Branch deleted on 2024-11-15 04:10_

---

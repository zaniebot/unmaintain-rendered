```yaml
number: 13332
title: "[red-knot] add initial Type::is_equivalent_to and Type::is_assignable_to"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/assignable
created_at: 2024-09-12T02:20:05Z
updated_at: 2024-09-12T18:15:28Z
url: https://github.com/astral-sh/ruff/pull/13332
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] add initial Type::is_equivalent_to and Type::is_assignable_to

---

_@carljm_

These are quite incomplete, but I needed to start stubbing them out in order to build and test declared-types.

Allowing unused for now, until they are used later in the declared-types PR.


---

_Label `red-knot` added by @carljm on 2024-09-12 02:22_

---

_Comment by @github-actions[bot] on 2024-09-12 02:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @carljm on 2024-09-12 02:44_

---

_Review requested from @MichaReiser by @carljm on 2024-09-12 02:44_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-12 02:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:328 on 2024-09-12 09:07_

nit: I know it can be tedious to list out all the missing branches here, but it means that it's guaranteed that we won't forget to update the method when we add new `Type` variants, so I'd prefer it

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:636 on 2024-09-12 09:09_

is it worth special-casing `builtins` specifically? We'll also need a more general-purpose way of querying if a module `X` has name `Y` and search-path `Z` for e.g. symbols from the `typing` modules -- and the `typing` module is *not* special-cased by the module resolver

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:755 on 2024-09-12 09:10_

what's the reason for the clippy ignore here?

---

_@AlexWaygood approved on 2024-09-12 09:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:300 on 2024-09-12 11:40_

I would remove the comment because it doesn't provide more information than the function signature or expand it to explain what assignability means. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:302 on 2024-09-12 11:42_

Is there a name that makes it more specific name than `other` that helps clarify the order of assignment (self is assigned to `other`?. Maybe `target`?

```suggestion
    pub(crate) fn is_assignable_to(self, db: &'db dyn Db, target: Type<'db>) -> bool {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:305 on 2024-09-12 11:43_

I would move the `is_equivalent` into the default branch on line 324 so that we can get a full exhaustiveness check. Except if calling `is_equivalent` first is good for perf

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:635 on 2024-09-12 11:45_

Doesn't `name` have a `PartialEq<str>` implementation

```suggestion
                .is_some_and(|module| module.name() == "builtins")
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:755 on 2024-09-12 11:46_

It's probably because `Ty` is passed by value to test-functions that don't consume it

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:841 on 2024-09-12 11:47_

Can you try if this works


```suggestion
    #[test_case(&Ty::BuiltinInstance("int"), &Ty::IntLiteral(1))]
    fn is_not_assignable_to(from: &Ty, to: &Ty) {
```

---

_@MichaReiser approved on 2024-09-12 11:47_

---

_@AlexWaygood reviewed on 2024-09-12 12:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:300 on 2024-09-12 12:08_

Maybe a better comment would link to the typing spec section on assignability

---

_@carljm reviewed on 2024-09-12 12:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:302 on 2024-09-12 12:58_

I thought the method name `is_assignable_to` already made the directionality quite clear? But I don't mind renaming the argument to `target`. 

---

_@carljm reviewed on 2024-09-12 13:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:305 on 2024-09-12 13:00_

I did it for performance reasons, so that the first thing we check is simple Eq of the types, before doing any more unnecessary work. (But I haven't done any benchmarking to verify that this makes a real difference.)

---

_@carljm reviewed on 2024-09-12 13:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:328 on 2024-09-12 13:06_

I don't think this idea works when we're talking about a double-match like this one. I definitely don't think we want to list out every possible pair of Type variants; we'd end up with a match statement of n^2 size, with most arms just returning false.

The inherent nature of assignability is that the default case for two unrelated type variants is "not assignable". I think it makes sense for the match statement to reflect this, and handle only the cases where they are or might be assignable.

---

_@carljm reviewed on 2024-09-12 13:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:755 on 2024-09-12 13:08_

Yeah; the alternative is sprinkling a bunch of annoying extra `&` in front of every Ty in every test case, which IMO harms readability for no actual gain. I can add a comment here explaining why this is here, I thought about doing that. 

---

_@carljm reviewed on 2024-09-12 13:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:841 on 2024-09-12 13:09_

It does work (I already tried it when writing the PR) but I don't like it, it's just useless noise when writing or reading the test cases. I would rather suppress the lint. There's no actual problem with the current code. 

---

_@carljm reviewed on 2024-09-12 13:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:636 on 2024-09-12 13:13_

Eh, probably not :) I was just doing the minimal needed for this PR. I can look at upgrading this to be able to identify any name from any stdlib module. 

---

_@AlexWaygood reviewed on 2024-09-12 13:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:328 on 2024-09-12 13:22_

Okay, fair enough!

---

_@AlexWaygood reviewed on 2024-09-12 13:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:841 on 2024-09-12 13:24_

I'm fine with suppressing the clippy lint personally as long we add a comment saying why we're doing it

---

_@carljm reviewed on 2024-09-12 13:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:755 on 2024-09-12 13:51_

I figured out how to remove the clippy suppression by changing the `to_type` method to a consuming `into_type` method instead (with some help from @MichaReiser to realize that in order to make this work I had to have `Ty::Union` keep a vec instead of a boxed array).

---

_@carljm reviewed on 2024-09-12 13:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:841 on 2024-09-12 13:55_

Got rid of the clippy suppression without all the extra `&`!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:303 on 2024-09-12 14:02_

```suggestion
    /// Return true if this type is [assignable to] type `target`.
    ///
    /// [assignable to]: https://typing.readthedocs.io/en/latest/spec/concepts.html#the-assignable-to-or-consistent-subtyping-relation
```

---

_@AlexWaygood reviewed on 2024-09-12 14:03_

---

_Merged by @carljm on 2024-09-12 18:15_

---

_Closed by @carljm on 2024-09-12 18:15_

---

_Branch deleted on 2024-09-12 18:15_

---

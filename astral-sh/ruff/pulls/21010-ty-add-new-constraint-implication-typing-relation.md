```yaml
number: 21010
title: "[ty] Add new \"constraint implication\" typing relation"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/new-relation
created_at: 2025-10-21T01:54:25Z
updated_at: 2025-10-28T02:01:11Z
url: https://github.com/astral-sh/ruff/pull/21010
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Add new "constraint implication" typing relation

---

_Pull request opened by @dcreager on 2025-10-21 01:54_

This PR adds the new **_constraint implication_** relationship between types, aka `is_subtype_of_given`, which tests whether one type is a subtype of another _assuming that the constraints in a particular constraint set hold_.

For concrete types, constraint implication is exactly the same as subtyping. (A concrete type is any fully static type that is not a typevar. It can _contain_ a typevar, though — `list[T]` is considered concrete.)

The interesting case is typevars. The other typing relationships (TODO: will) all "punt" on the question when considering a typevar, by translating the desired relationship into a constraint set. At some point, though, we need to resolve a constraint set; at that point, we can no longer punt on the question. Unlike with concrete types, the answer will depend on the constraint set that we are considering.


---

_Label `ty` added by @dcreager on 2025-10-21 01:55_

---

_Label `internal` added by @dcreager on 2025-10-21 01:56_

---

_Comment by @github-actions[bot] on 2025-10-21 01:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 01:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1 on 2025-10-23 19:42_

I had originally written this as a new `TypeRelation` variant, but that was too heavy-handed, given that constraint implication is exactly the same as subtyping for everything except for typevars.

---

_@dcreager reviewed on 2025-10-23 19:42_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of_given.md`:50 on 2025-10-24 17:48_

This TODO is #20093

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of_given.md`:177 on 2025-10-24 17:50_

This will be involved enough that I'm tackling it separately, in #21068 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:8537 on 2025-10-24 17:52_

We should always be comparing a typevar's identity, and this makes it harder to accidentally compare specific instances of a typevar.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/type_ordering.rs`:144 on 2025-10-24 17:53_

but this is the one place where we do still want to compare typevar instances, not identities

---

_@dcreager reviewed on 2025-10-24 17:54_

---

_Marked ready for review by @dcreager on 2025-10-24 18:19_

---

_Review requested from @carljm by @dcreager on 2025-10-24 18:19_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-24 18:19_

---

_Review requested from @sharkdp by @dcreager on 2025-10-24 18:19_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-24 18:19_

---

_Review comment by @MichaReiser on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:82 on 2025-10-27 09:33_

I just wanted to suggest making this a method on `ConstraintSet` but that doesn't work because you also want to allow `bool` as `constraints`. 

I'm not sure I fully understand the semantics here. Is this roughly if and only if `constraints` semantics? If that's the case, I think I'd find `is_subtype_of_iff(ty, of, constraints)` easier to understand.

But I don't think that's what this API does after reading the mdtests. Hmm :)


I'm asking because I first assumed the relation between `is_subtype_of` and `is_subtype_of_given` is similar to `std::mem::size_of` and `std::mem::size_of_val` where the latter takes a value. I don't think this is the case here because `constraints` would than have to be a subtype of `ty`, which the API doesn't require.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of_given.md`:26 on 2025-10-27 09:35_

Is `is_subtype_of(T, V)` guaranteed to always be the same as `is_subtype_of(True, T, V)`. If so, maybe that's something you could add to the documentation of `is_subtype_of_given`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:4852 on 2025-10-27 09:40_

Is it intentional that the first two parameters have the same name? Should we use the same names as in the `pyi` files (it would allow me to easier match the parameters)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:188 on 2025-10-27 09:41_

Is it? ;)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:360 on 2025-10-27 09:44_

Should this use `is_same_typevar`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/type_ordering.rs`:144 on 2025-10-27 09:47_

Should we add an inline comment explaining why that's the case?

---

_@MichaReiser approved on 2025-10-27 09:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of_given.md`:26 on 2025-10-27 19:03_

This is a great catch! I was in the middle of writing that this would not be true when comparing typevars — but you're right, and it _is_ true even then:

```py
is_subtype_of(bool, int)               # yes
is_subtype_of_given(True, bool, int)   # yes
is_subtype_of_given(False, bool, int)  # yes

def f[T]():
    is_subtype_of(T, int)              # no (T could be anything!)
    is_subtype_of_given(True, T, int)  # also no (T could be anything!)
```

This also made me realize that I don't need to (and shouldn't) project away the other typevars like I was doing.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:188 on 2025-10-27 19:07_

@AlexWaygood do you have thoughts here? I was thinking that we should treat redundancy the same as subtyping here. Adding more context, the question is, if you call `is_redundant_with(T, list[Any])`, what constraint should we turn that into? For `is_subtype_of(T, list[Any])`, we turn it into `T ≤ Bottom[list[Any]]`, since for subtyping, the relationship has to hold for _all_ materializations. For `is_assignable_to(T, list[Any])`, we turn it into `T ≤ Top[list[Any]]`, since the relationship only has to hold for _some_ materialization. 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:360 on 2025-10-27 19:08_

Yes!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/type_ordering.rs`:144 on 2025-10-27 19:08_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4852 on 2025-10-27 19:10_

Nope, that was bad copy/pasta. Done

(And once we have support for `TypeForm`, all of these special cases can go away)

---

_Review comment by @dcreager on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:82 on 2025-10-27 19:12_

> I just wanted to suggest making this a method on `ConstraintSet` but that doesn't work because you also want to allow `bool` as `constraints`.

Yeah, I thought the same thing, but our `KnownFunction` machinery is focused on top-level functions in a module. I had considered adding support for "known methods of a class", but that seemed like it would be out of scope for this PR. I can open an issue to track that.

> I'm not sure I fully understand the semantics here. Is this roughly if and only if `constraints` semantics? If that's the case, I think I'd find `is_subtype_of_iff(ty, of, constraints)` easier to understand.

I can copy over more of the commentary from the mdtest to explain the method better. 

---

_@dcreager reviewed on 2025-10-27 19:17_

---

_@AlexWaygood reviewed on 2025-10-27 19:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:188 on 2025-10-27 19:48_

Happy to take a look at this, but might not get to it until Wednesday as I'm busy travelling tomorrow. Don't feel you have to wait for me before merging!

In general, if we're in doubt about how redundancy should behave, the safe thing to do in that situation is always to treat it like subtyping

---

_@dcreager reviewed on 2025-10-27 19:56_

---

_Review comment by @dcreager on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:82 on 2025-10-27 19:56_

Done. I put the expanded documentation on the Rust side, to be consistent with where we are documenting the other typing internals.

---

_@dcreager reviewed on 2025-10-27 20:02_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:188 on 2025-10-27 20:02_

Thanks! This isn't hooked up to anything yet except isolated mdtests, so no urgency.

---

_@dcreager reviewed on 2025-10-27 20:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:188 on 2025-10-27 20:03_

(But I will still probably merge this and address anything you might find in a follow-on PR)

---

_@dcreager reviewed on 2025-10-27 20:09_

---

_Review comment by @dcreager on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:82 on 2025-10-27 20:09_

> Yeah, I thought the same thing, but our `KnownFunction` machinery is focused on top-level functions in a module. I had considered adding support for "known methods of a class", but that seemed like it would be out of scope for this PR. I can open an issue to track that.

https://github.com/astral-sh/ty/issues/1447

---

_Merged by @dcreager on 2025-10-28 02:01_

---

_Closed by @dcreager on 2025-10-28 02:01_

---

_Branch deleted on 2025-10-28 02:01_

---

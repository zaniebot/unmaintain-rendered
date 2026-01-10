```yaml
number: 17023
title: "[red-knot] Allow explicit specialization of generic classes"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/special-class
created_at: 2025-03-27T20:25:22Z
updated_at: 2025-05-07T15:19:54Z
url: https://github.com/astral-sh/ruff/pull/17023
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Allow explicit specialization of generic classes

---

_Pull request opened by @dcreager on 2025-03-27 20:25_

This PR lets you explicitly specialize a generic class using a subscript expression. It introduces three new Rust types for representing classes:

- `NonGenericClass`
- `GenericClass` (not specialized)
- `GenericAlias` (specialized)

and two enum wrappers:

- `ClassType` (a non-generic class or generic alias, represents a class _type_ at runtime)
- `ClassLiteralType` (a non-generic class or generic class, represents a class body in the AST)

We also add internal support for specializing callables, in particular function literals.  (That is, the internal `Type` representation now attaches an optional specialization to a function literal.) This is used in this PR for the methods of a generic class, but should also give us most of what we need for specializing generic _functions_ (which this PR does not yet tackle).

---

_Label `red-knot` added by @dcreager on 2025-03-27 20:25_

---

_Comment by @github-actions[bot] on 2025-03-27 20:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/async-utils/src/async_utils/scheduler.py:75:38: Cannot subscript object of type `Literal[_Task]` with no `__class_getitem__` method
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/scheduler.py:148:30: Object of type `T` cannot be assigned to parameter 3 (`payload`) of bound method `__init__`; expected type `T`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/_misc/_queue_testing.py:131:24: Object of type `None` cannot be assigned to parameter 2 (`item`) of bound method `sync_put`; expected type `T`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/async-utils/src/async_utils/_qs.py:510:16: Cannot subscript object of type `Literal[BaseQueue]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/async-utils/src/async_utils/_qs.py:538:20: Cannot subscript object of type `Literal[BaseQueue]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/async-utils/src/async_utils/_qs.py:566:24: Cannot subscript object of type `Literal[BaseQueue]` with no `__class_getitem__` method
- Found 38 diagnostics
+ Found 32 diagnostics

```
</details>


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:84 on 2025-04-07 22:32_

The diagnostics look like this because I'm reusing the call binding mechanism to match parameters and check types. It would be nice for this diagnostic to talk about _type_ parameters, but would prefer to do that as a follow-on.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:201 on 2025-04-07 22:36_

This depends on https://github.com/salsa-rs/salsa/pull/741

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:273 on 2025-04-07 22:45_

Most of these methods exist on both `ClassType` and `ClassLiteralType`.  The "real" implementations are on `ClassLiteralType`, since that's what represents a class body in the AST.  The `ClassType` methods wrap that, applying the specialization in the case of a generic alias.

---

_@dcreager reviewed on 2025-04-07 22:46_

Other than the tracked structs Salsa bug, this is ready for review!

---

_Marked ready for review by @dcreager on 2025-04-07 22:52_

---

_Review requested from @carljm by @dcreager on 2025-04-07 22:52_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-07 22:52_

---

_Review requested from @sharkdp by @dcreager on 2025-04-07 22:52_

---

_@sharkdp reviewed on 2025-04-08 13:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:104 on 2025-04-08 13:21_

I guess it's not that important (it's an error anyway), but `Constrained[Unknown]` might be a better fallback?

---

_@sharkdp reviewed on 2025-04-08 13:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:105 on 2025-04-08 13:25_

I don't think we necessarily have to do something about this, but it's interesting that this does not work at runtime (raises `AttributeError`). Probably because `type(C[int])` is a `_GenericAlias`, so the static lookup on the meta type does not find the `f` attribute.

---

_Comment by @sharkdp on 2025-04-08 13:36_

> and two enum wrappers:
> 
>     * `ClassType` (a non-generic class or generic alias, represents a class _type_ at runtime)

Minor comment: is this outdated or am I misunderstanding something? Are you talking about `Type` enum variants here? If so, should it be `Type::GenericAlias`?

If this interpretation is correct, that would mean that non-generic class types also fall under `Type::GenericAlias` (which might be fine, I'm just trying to understand).

---

_Comment by @dcreager on 2025-04-08 13:59_

> Minor comment: is this outdated or am I misunderstanding something? Are you talking about `Type` enum variants here? If so, should it be `Type::GenericAlias`?
> 
> If this interpretation is correct, that would mean that non-generic class types also fall under `Type::GenericAlias` (which might be fine, I'm just trying to understand).

No, it (and `ClassLiteralType`) are new enums defined in `types/class.rs`:

https://github.com/astral-sh/ruff/blob/43554e2ac9d7d7c0084bc34b56cb573370fecbdb/crates/red_knot_python_semantic/src/types/class.rs#L170-L173

`Type::ClassLiteral` wraps `ClassLiteralType`.  `ClassType` doesn't have a `Type` variant itself; instead it's used in e.g. `Type::Instance` and `Type::SubclassOf`, since an object is an instance of a non-generic class or of a specialized generic alias.

So a non-generic class (i.e. the class itself, not an instance of it) would be a

```rust 
Type::ClassLiteral(ClassLiteralType::NonGeneric(NonGenericClass { .. }))
```

A generic class that hasn't been specialized would be a 

```rust
Type::ClassLiteral(ClassLiteralType::Generic(GenericClass { .. }))
```

and a generic class that has been specialized would be a 

```rust
Type::GenericAlias(GenericAlias { .. })
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:105 on 2025-04-08 14:37_

There's a lot of magic in the runtime implementation of generics in the `typing` module, and there will be some judgment calls about how much to model this precisely and how much to maintain the ideal fiction that `_GenericAlias` attempts to approximate with its proxying behavior.

The main purpose of our special handling of `getattr_static` is for our own tests, and for that purpose I think it's more useful for it to reflect the "ideal" specialized generic class, not the real runtime behavior of `getattr_static`. There could be an argument that we should make our own function in `knot_extensions` for our test purposes instead of reusing `inspect.getattr_static`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:100 on 2025-04-08 14:42_

I think we are trying to move away from these bracketed type displays and towards more Pythonish syntax, see discussion in https://github.com/astral-sh/ruff/pull/17294. I think the natural choice here would be e.g. `foo[int]` instead of `<foo specialized with T = int>`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:201 on 2025-04-08 14:48_

If we want to unblock this PR on the salsa fix, the other thing we can do is temporarily revert the change that makes mdtests reuse the same Salsa db, and live with slow mdtests until the Salsa fix lands.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:14 on 2025-04-08 14:51_

`Foo[Bar]` should be the type of an _instance_ of the specialized generic class `Foo[Bar]`, not the class-literal type that should appear in the MRO. I think we should see `Literal[Foo[Bar]]` here? (At least until/unless we change our display of class literal types to something other than `Literal[C]`).

---

_@carljm reviewed on 2025-04-08 15:05_

This is fantastic!

Haven't reviewed nearly all of it yet, but I'm going to switch focus to the Salsa bug, so submitting the comments I have.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/generics.rs`:52 on 2025-04-08 21:37_

What is your longer-term thinking about the reuse of `Signature` and call-binding for specialization? It seems to me that it maybe doesn't buy us very much, since type params are always positional-only, meaning we don't need most of the interesting code from call binding, and it will be hard to integrate e.g. correct handling of constrained typevars, and better diagnostics. (I guess what I'm saying here is I can see why this was a quick way to get something working, but I'm not convinced this is a path we want to go down, vs just writing separate custom code for specialization.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/generics.rs`:70 on 2025-04-08 21:40_

And also not the union of them.

(We wouldn't necessarily need a "new type variant" to handle this if we had bespoke code for specialization instead of reusing call binding.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4175 on 2025-04-09 02:19_

Can we add a comment here explaining why we don't need to apply specialization to the `self_instance` type of the bound method?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3979 on 2025-04-09 02:23_

I think it would also be useful to have explicit comments here (or in the doc comment above) about why the `apply_specialization` operation doesn't apply to `ClassLiteral`, `SubclassOf`, or `Instance` types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5339 on 2025-04-09 02:31_

If we have a generic method in a generic class, say

```py
class C[T]:
    def method[U](self, t: T, u: U) -> U: ...
```

If we access the method off of an instance of `C[int]`, we'd apply the specialization `T = int` to the returned bound method, and then at the call site we would apply a further specialization of, say, `U = str`? And then check the actual call against it with a specialization of `T = int, U = str`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:501 on 2025-04-09 03:34_

I'm not sure this is correct/safe. If we have a narrowing predicate `type(x) is C` and `C` is a generic class, that does not necessarily mean that `x` is an instance of the default specialization of `C`. It means that `x` is an instance of _some_ specialization of `C`, but I don't think we can glean any information from the predicate expression about which one.

If the default specialization is simply `Unknown` for all typevars, maybe this works out OK? Since `C[Unknown]` expresses "an instance of some specialization of `C`, but we don't know which one." But if any of the typevars of `C` actually have defaults, it's not clear to me that we should apply those defaults in this case. Typevar defaults are intended to save unnecessary typing when spelling a type in a type expression, not to make unsafe assumptions when interpreting runtime narrowing from the use of the generic class as a value expression.

We can just add a TODO from this for now, but I suspect we should have a variant of "default specialization" which ignores typevar defaults and uses `Unknown`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests/type_generation.rs`:172 on 2025-04-09 03:36_

Probably we should have a TODO for better coverage of generic types (including non-default specializations) in the property tests?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1738 on 2025-04-09 03:47_

I think this TODO should at least be updated in light of the code below (sort of) handling this case now

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1741 on 2025-04-09 03:53_

Can be a TODO for now, but I suspect that deferring _all_ inference of class bases if any of them contain any string literal is too big a hammer here, and we will need to do something more complex. In a case like this:

```py
class B[T]: pass

class C(B["Forward"]): pass

class Forward: pass

B = "oops"
```

We have to infer the type of `B` at the point of use (and not pick up the later binding of `B`), but we have to use a deferred lookup for `"Forward"`. This will be a bit awkward, as it means we have to keep track of unspecialized `B` somewhere, until we actually resolve `"Forward"` and can specialize.

---

_@carljm approved on 2025-04-09 03:59_

This is great!!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:104 on 2025-04-09 13:47_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:105 on 2025-04-09 13:51_

https://github.com/astral-sh/ruff/issues/17312

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:14 on 2025-04-09 13:53_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4175 on 2025-04-09 14:00_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3979 on 2025-04-09 14:04_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:5339 on 2025-04-09 14:08_

Yes, that's the intent! I can only verify the correct behavior as part of #17301, but I've added an xfail test for it here.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/display.rs`:100 on 2025-04-09 14:24_

Done (ditto for `bound method` and `method-wrapper` below)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/generics.rs`:52 on 2025-04-09 14:31_

That's a good point, though my original thinking was to make it lean on the existing call binding mechanism _more_:

https://github.com/astral-sh/ruff/blob/43554e2ac9d7d7c0084bc34b56cb573370fecbdb/crates/red_knot_python_semantic/src/types/infer.rs#L5692-L5695

i.e. explicit specialization would happen in a custom callable, which `__getitem__` would resolve to on generic classes. Though that would require adding hooks to `Signature` to allow customized diagnostics.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/generics.rs`:70 on 2025-04-09 14:33_

Done.  And that's a good point in favor of special logic instead of a custom callable for `__getitem__`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1738 on 2025-04-09 14:35_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1741 on 2025-04-09 14:36_

Added a TODO. I'll have to learn more about the deferral mechanism to be able to tackle that.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/narrow.rs`:501 on 2025-04-09 15:05_

Done. This also made me realize that we weren't narrowing when comparing against a generic alias, which I also fixed.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/property_tests/type_generation.rs`:172 on 2025-04-09 15:07_

Done

---

_@dcreager reviewed on 2025-04-09 15:07_

---

_Merged by @dcreager on 2025-04-09 15:18_

---

_Closed by @dcreager on 2025-04-09 15:18_

---

_Branch deleted on 2025-04-09 15:18_

---

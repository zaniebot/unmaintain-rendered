```yaml
number: 17168
title: "[red-knot] Three-argument type-calls take 'str' as the first argument"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/first-type-argument-should-be-str
created_at: 2025-04-03T08:55:18Z
updated_at: 2025-04-03T13:45:09Z
url: https://github.com/astral-sh/ruff/pull/17168
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Three-argument type-calls take 'str' as the first argument

---

_Pull request opened by @sharkdp on 2025-04-03 08:55_

## Summary

Similar to #17163, a minor fix in the signature of `type(…)`.

## Test Plan

New MD tests

---

_Label `red-knot` added by @sharkdp on 2025-04-03 08:55_

---

_Review requested from @carljm by @sharkdp on 2025-04-03 08:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-03 08:55_

---

_Review requested from @dcreager by @sharkdp on 2025-04-03 08:55_

---

_Comment by @github-actions[bot] on 2025-04-03 08:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:238:42: Object of type `tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:327:39: Object of type `tuple[@Todo(Support for `typing.TypeVar` instances in type expressions), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
- Found 97 diagnostics
+ Found 95 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2903 on 2025-04-03 10:54_

for now we could do

```suggestion
                                        // TODO: Should be tuple[type, ...] once we have support for homogenous tuples
                                        .with_annotated_type(KnownClass::Tuple.to_instance(db)),
                                    Parameter::positional_only(Some(Name::new_static("dict")))
                                        // TODO: Should be `dict[str, Any]` once we have support for generics
                                        .with_annotated_type(KnownClass::Dict.to_instance(db)),
```

but it doesn't really matter that much

---

_@AlexWaygood approved on 2025-04-03 10:54_

---

_Comment by @sharkdp on 2025-04-03 11:46_

> ```diff
> + error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/decorator.py:380:27: No overload of class `type` matches arguments
> ```

Hm, now we have a false positive, because we don't understand that `tuple[Any]` (or more specifically, `tuple[@Todo, @Todo]`, in this example) is assignable to `tuple`. Maybe because we rely on subtyping in order for `x -> tuple` assignments to work? I'll look into it.

---

_Comment by @AlexWaygood on 2025-04-03 11:52_

> > ```diff
> > + error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/decorator.py:380:27: No overload of class `type` matches arguments
> > ```
> 
> Hm, now we have a false positive, because we don't understand that `tuple[Any]` (or more specifically, `tuple[@Todo, @Todo]`, in this example) is assignable to `tuple`. Maybe because we rely on subtyping in order for `x -> tuple` assignments to work? I'll look into it.

ah, it looks like we understand that `tuple[int, int]` is a subtype of `tuple` and therefore assignable to `tuple`, but perhaps we don't understand that `tuple[@Todo, @Todo]` is assignable to `tuple` (because it is not a subtype of `tuple`). So we probably need to add a fallback (Type::Tuple(_), _) => KnownClass::Tuple.to_instance(db).is_assignable_to(target)` branch to `Type::is_assignable_to()`, beneath the existing `(Type::Tuple(), Type::Tuple())` branch

---

_Comment by @sharkdp on 2025-04-03 11:56_

> So we probably need to add a fallback (Type::Tuple(_), _) => KnownClass::Tuple.to_instance(db).is_assignable_to(target)`branch to`Type::is_assignable_to()`, beneath the existing `(Type::Tuple(), Type::Tuple())` branch

That's exactly what I did, yes :+1:. The false positive is gone.

---

_@AlexWaygood reviewed on 2025-04-03 12:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:960 on 2025-04-03 12:00_

I think we can generalise this: for any two types `T` and `X` where `T` is a heterogeneous tuple type and `X` is an arbitrary type, if `tuple` is assignable to `X` then `T` is also assignable to `X`, since any heterogeneous tuple type `T` will always be assignable to `tuple`, and the assignability relation is transitive

```suggestion
            (Type::Tuple(_), _) => KnownClass::Tuple.to_instance(db).is_assignable_to(target),
```

---

_@sharkdp reviewed on 2025-04-03 12:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:960 on 2025-04-03 12:49_

Turns out that was not exactly what I did :smile:. Thanks for the suggestion — good call!

---

_@AlexWaygood approved on 2025-04-03 12:51_

LG! Don't merge right now, though, there's a patch release in progress 😄

---

_Comment by @AlexWaygood on 2025-04-03 12:51_

> ```diff
> - error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:238:42: Object of type `tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
> - error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:327:39: Object of type `tuple[@Todo(Support for `typing.TypeVar` instances in type expressions), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
> ```

and the new branch in `Type::is_assignable_to()` now gets _rid_ of some false positives 🥳 because `tuple` is assignable to `Sequence`, so therefore `tuple[@Todo, @Todo]` is assignable to `Sequence` also

---

_Merged by @sharkdp on 2025-04-03 13:45_

---

_Closed by @sharkdp on 2025-04-03 13:45_

---

_Branch deleted on 2025-04-03 13:45_

---

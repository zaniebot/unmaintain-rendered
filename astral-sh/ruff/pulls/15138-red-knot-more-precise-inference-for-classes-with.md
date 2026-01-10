```yaml
number: 15138
title: "[red-knot] More precise inference for classes with non-class metaclasses"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-metaclasses
created_at: 2024-12-25T00:30:53Z
updated_at: 2025-01-09T00:44:20Z
url: https://github.com/astral-sh/ruff/pull/15138
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] More precise inference for classes with non-class metaclasses

---

_Pull request opened by @InSyncWithFoo on 2024-12-25 00:30_

## Summary

Resolves #14208.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-25 00:30_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-25 00:30_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-25 00:30_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-25 00:30_

---

_Comment by @github-actions[bot] on 2024-12-25 00:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-12-25 00:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3399 on 2024-12-29 00:12_

I think we should use `return_ty_result` here for better handling of union cases?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:188 on 2024-12-29 00:12_

I think this should emit an `invalid-metaclass` diagnostic.

---

_@carljm reviewed on 2024-12-29 00:13_

---

_@InSyncWithFoo reviewed on 2024-12-29 03:10_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:3399 on 2024-12-29 03:10_

I'm not sure if `.return_ty_result()` is optimal in this context. It adds diagnostics all on its own, but such errors should instead be delegated so that `INVALID_METACLASS` can be emitted cleanly. This would require either:

* Handling call outcomes here, one by one, instead of asking `.return_ty_result()`. Problem: Code duplication.
* Add a new method `.return_ty_result_pure()` (tentative name) that does not report diagnostics. Problem: One-time refactoring cost (`.return_ty_result()` is currently used at 8 places).

I personally prefer the former, because it is simpler and allows for minutiae control. Besides, I don't think `class A(metaclass=reveal_type)` should trigger the "reveal type" effect.

---

_@carljm reviewed on 2024-12-30 00:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3399 on 2024-12-30 00:35_

Hmm, our `CallOutcome` methods are in a bit of a confused state, I think tbh some refactoring is needed :/ For non-union error cases, `return_ty_result` does not emit a diagnostic (it just returns an error result), but it does for `reveal_type` and for union cases. But we don't need to refactor this in this PR.

I'm fine with directly matching on the `CallOutcome` here for now, to avoid scope creep into refactoring `CallOutcome` methods.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:651 on 2025-01-08 17:21_

```suggestion
                            "Metaclass type `{}` may not be callable",
```

---

_@carljm approved on 2025-01-08 17:21_

Looks great, thank you! Will apply my diagnostic wording nit and then merge.

---

_Comment by @carljm on 2025-01-08 17:29_

Oops, looks like this needs a rebase before it can land. I don't have time to do that right now, but will merge if you're able to do it.

---

_Comment by @carljm on 2025-01-08 18:28_

Sorry, I should have clarified that it's not just a rebase -- the rebase causes logical conflicts that make compilation fail.

---

_Comment by @InSyncWithFoo on 2025-01-08 20:42_

> [...] the rebase causes logical conflicts that make compilation fail.

Indeed, now there's a panic that I can't make heads or tails of:

```text
metaclass.md - Non-class

  crates/red_knot_python_semantic/resources/mdtest/metaclass.md:169 panicked at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/runtime.rs:335:13
```

Here's the code in question:

```python
def f(*args, **kwargs) -> int: ...  # 169 (not even modified)

class A(metaclass=f): ...           # 171 (also not modified)
```

I'm also getting ```"Module `knot_extensions` has no member `static_assert`"``` errors, but only locally (Windows). Does the new Python API require some kind of configuration?

---

_Comment by @carljm on 2025-01-08 20:51_

> now there's a panic

I can repro this, will take a couple minutes to look at it and see if I can understand it.

> I'm also getting `` "Module `knot_extensions` has no member `static_assert`" `` errors, but only locally (Windows). Does the new Python API require some kind of configuration?

No, but I suspect this is related to the use of a symlink to include `knot_extensions.pyi` in typeshed; we should probably just copy instead. Cc @sharkdp 

---

_Comment by @carljm on 2025-01-08 21:07_

The panic is due to a Salsa query cycle. Still looking into why it's new after the rebase.

---

_Comment by @sharkdp on 2025-01-08 21:17_

> No, but I suspect this is related to the use of a symlink to include `knot_extensions.pyi` in typeshed; we should probably just copy instead. Cc @sharkdp

Strange that our CI tests on Windows seem to have no problem with it? I can take a look tomorrow.

---

_Comment by @carljm on 2025-01-09 00:19_

Ok, I tracked down the cycle. It's new with the rebase because we are now doing proper call binding. What happens is that we call the metaclass `f`, which takes `*args`, and the behavior of call binding is to create a bound type for the variadic parameter `*args` which is the union of the types of all arguments which map to that variadic parameter. In this case that is all three of `self_ty`, `bases`, and `namespace`. In constructing that union, we end up checking whether `namespace` (which is `dict`) and `self_ty` (which is the class `A`) are subtypes of each other, and this check ends up needing to know the metaclass of `A`, which obviously leads to a cycle.

I think this reveals a bug in the PR that I hadn't noticed. The first argument to the metaclass is not the actual class object, as modeled in the PR via `self_ty`. That would be pretty circular at runtime, too, since the metaclass is responsible for creating the class object! The first argument to the metaclass is a string, the name of the new class:

```
>>> def f(*a):
...     print(a)
...
>>> class A(metaclass=f): ...
...
('A', (), {'__module__': '__main__', '__qualname__': 'A', '__firstlineno__': 1, '__static_attributes__': ()})
```

If we correctly reflect this by passing `Type::string_literal(db, self.name(db))` as the first argument instead of `Type::class_literal(self)`, the cycle goes away and the tests pass.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3616 on 2025-01-09 00:20_

```suggestion
            let name = Type::string_literal(self.name(db));
            let bases = TupleType::from_elements(db, self.explicit_bases(db));
            // TODO: Should be `dict[str, Any]`
            let namespace = KnownClass::Dict.to_instance(db);

            // TODO: Other keyword arguments?
            let arguments = CallArguments::positional([name, bases, namespace]);
```

---

_@carljm reviewed on 2025-01-09 00:20_

---

_@carljm reviewed on 2025-01-09 00:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3610 on 2025-01-09 00:22_

```suggestion
            let name = Type::string_literal(db, self.name(db));
```

---

_Comment by @carljm on 2025-01-09 00:25_

For future reference, the technique I found most effective for tracking down the cause of the cycle was simply adding `dbg!` statements in the red-knot code to bisect where the panic occurred (I found it occurred in the `metaclass.call(...)` line in the non-class metaclass branch of `Class::try_metaclass`, then I added `dbg!` statements inside `Type::call` and discovered it was happening inside `bind_call`, then I added `dbg!` statements inside `bind_call` until I figured out that it was happening when adding the third argument to the union of argument types for `*args`, etc.)

Using query tracing (in red-knot or Salsa) was ineffective in this case, because the cycle was a single-query cycle: it was just `Class::try_metaclass` calling itself; there were some intervening function call layers, but no intervening Salsa queries.

---

_@carljm reviewed on 2025-01-09 00:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3660 on 2025-01-09 00:28_

```suggestion
                // TODO we should also check for binding errors that would indicate the metaclass
                // does not accept the right arguments
                CallOutcome::Callable { binding }
```

---

_Merged by @carljm on 2025-01-09 00:34_

---

_Closed by @carljm on 2025-01-09 00:34_

---

_Branch deleted on 2025-01-09 00:44_

---

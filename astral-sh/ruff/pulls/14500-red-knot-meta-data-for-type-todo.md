```yaml
number: 14500
title: "[red-knot] Meta data for `Type::Todo`"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/todo-type-metadata
created_at: 2024-11-20T20:44:07Z
updated_at: 2024-11-21T08:59:50Z
url: https://github.com/astral-sh/ruff/pull/14500
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Meta data for `Type::Todo`

---

_@sharkdp_

## Summary

Adds meta information to `Type::Todo`, allowing developers to easily trace back the origin of a particular `@Todo` type they encounter.

Instead of `Type::Todo`, we now write either `type_todo!()` which creates a `@Todo[path/to/source.rs:123]` type with file and line information, or using `type_todo!("PEP 604 unions not supported")`, which creates a variant with a custom message.

`Type::Todo` now contains a `TodoType` field. In release mode, this is just a zero-sized struct, in order not to create any overhead. In debug mode, this is an `enum` that contains the meta information.

`Type` implements `Copy`, which means that `TodoType` also needs to be copyable. This limits the design space. We could intern `TodoType`, but I discarded this option, as it would require us to have access to the salsa DB everywhere we want to use `Type::Todo`. And it would have made the macro invocations less ergonomic (requiring us to pass `db`).

So for now, the meta information is simply a `&'static str` / `u32` for the file/line variant, or a `&'static str` for the custom message. Anything involving a chain/backtrace of several `@Todo`s or similar is therefore currently not implemented. Also because we currently don't see any direct use cases for this, and because all of this will eventually go away.

Note that the size of `Type` increases from 16 to 24 bytes, but only in debug mode.

## Test Plan

- Observed the changes in Markdown tests.
- Added custom messages for all `Type::Todo`s that were revealed in the tests
- Ran red knot in release and debug mode on the following Python file:
  ```py
  def f(x: int) -> int:
      reveal_type(x)
  ```
  Prints `@Todo` in release mode and `@Todo(function parameter type)` in debug mode.


---

_Label `internal` added by @sharkdp on 2024-11-20 20:44_

---

_Label `red-knot` added by @sharkdp on 2024-11-20 20:44_

---

_Review requested from @carljm by @sharkdp on 2024-11-20 20:44_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-20 20:44_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-20 20:44_

---

_Comment by @github-actions[bot] on 2024-11-20 20:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:399 on 2024-11-20 20:59_

```suggestion
    /// This variant should be created with the [`todo_type!`] macro.
```

---

_@MichaReiser approved on 2024-11-20 21:02_

Clever!

---

_@sharkdp reviewed on 2024-11-20 21:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:399 on 2024-11-20 21:46_

I reverted this, as `cargo doc` failed. `todo_type!` is only `pub(crate)`, but `Type` is `pub`. I don't know if it would be fine to make `Type` `pub(crate)`, and it's not straightforward anyway. And I don't really want to make the macro `pub`, as that would require using `#[macro_export]` and `$crate` in both variants of the macro... all of which seems like a bit too much for this small QOL improvement.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:327 on 2024-11-20 21:51_

Just to clarify that this is for red-knot implementation limitations, not for limitations in the Python type specification.

```suggestion
/// Meta data for `Type::Todo`, which represents a known limitation in red-knot.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1239 on 2024-11-20 21:56_

I guess there are some judgment calls about when to propagate a Todo vs create a new one. This could propagate, like we do below in `to_instance` and `to_meta_type`, but maybe an attribute of a todo feels more like a new todo than an extension of the previous one? Not sure what will be more useful in practice.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3655 on 2024-11-20 22:02_

I think these are all not actually correct (as discussed in Discord today), but that's orthogonal to this PR; these assertions are consistent with our current handling. So I think it makes sense to leave them as-is for this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3669 on 2024-11-20 22:03_

```suggestion
        // And similar for intersection types:
```

---

_@carljm approved on 2024-11-20 22:05_

This is great! Will make debugging so much nicer.

---

_@sharkdp reviewed on 2024-11-20 22:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3655 on 2024-11-20 22:10_

Oh, I was about to read that discussion again tomorrow. I thought it was unrelated to my change :smile:. I can look into it (not here).

---

_@carljm reviewed on 2024-11-21 00:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3655 on 2024-11-21 00:43_

It is unrelated to your change, except that you added some explicit tests that `Todo` (which is supposed to be a dynamic type just like `Any` or `Unknown`) is equivalent to and a subtype of itself, which I don't think we had explicitly tested before, and is not really right. So that makes it related :)

---

_@sharkdp reviewed on 2024-11-21 08:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1239 on 2024-11-21 08:50_

I'm going to assume for now that the original todo is more interesting, so I changed the logic here to propagate the incoming todo.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3655 on 2024-11-21 08:53_

Okay yes, this is what I understood after your first comment. I'll address this in a follow-up and fix these tests accordingly.

---

_@sharkdp reviewed on 2024-11-21 08:53_

---

_Merged by @sharkdp on 2024-11-21 08:59_

---

_Closed by @sharkdp on 2024-11-21 08:59_

---

_Branch deleted on 2024-11-21 08:59_

---

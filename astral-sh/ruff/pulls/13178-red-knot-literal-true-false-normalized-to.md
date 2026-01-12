```yaml
number: 13178
title: "[red-knot] `Literal[True,False]` normalized to `builtins.bool`"
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: bool-true-false
created_at: 2024-08-31T03:42:03Z
updated_at: 2024-09-02T07:56:35Z
url: https://github.com/astral-sh/ruff/pull/13178
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] `Literal[True,False]` normalized to `builtins.bool`

---

_@dylwil3_

The `UnionBuilder` builds `builtins.bool` when handed `Literal[True]` and `Literal[False]`.

Caveat: If the builtins module is unfindable somehow, the builder falls back to the union type of these two literals.

First task from astral-sh/ty#245


---

_Review requested from @carljm by @dylwil3 on 2024-08-31 03:42_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-08-31 03:42_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-31 03:42_

---

_Comment by @dylwil3 on 2024-08-31 03:48_

Note: Attempting to call `builtins_symbol_ty_by_name` on a the default `TestDb` (without making a `src` directory and initializing a `Program`) causes a panic. Is this expected behavior? From the docstring I would've expected to just get `Type::Unbound`. The panic originates from `Program::get(db)` here:

https://github.com/astral-sh/ruff/blob/3ceedf76b8df1cfe749d0bb4c4f0aad6ee97ef6a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs#L545-L550

If that's unexpected I can open an issue.

---

_Comment by @github-actions[bot] on 2024-08-31 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:278 on 2024-08-31 04:34_

I think it would be fine to just add all this to the existing `setup_db` in this module. It's using a memory "file system" so nothing very slow is happening here. If the builders may now depend on Program, it's better to just have it in all tests and not have to think about which tests need what kind of setup. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-08-31 04:42_

So I should have been clearer in my description of the desired behavior; we want to not just ensure that we can't build a union containing exactly Literal[True] and Literal[False], we want to ensure that any union we build, even if it contains other types also, always replaces those two Literals, if both present, with bool. 

So let's add a test for the case where there is also another type in the union. 

To implement this, I think we'll probably want to add a `simplify` method that gets called before building the final union, similar to what we have for intersections. It can do a single pass over the elements, tracking whether it finds both Boolean literal types, and if so removing them both and adding bool. 

The other way to handle it might be upfront when types are added. This seems like it might be less efficient given the need to search the existing elements for the other literal bool type whenever a literal bool type is added to the union. But this is probably ok in practice, especially if we can do it only when a previously-not-present literal bool type is added to the union. I think whichever seems to work out better in the code can be fine. 

---

_@carljm requested changes on 2024-08-31 04:46_

Thank you!!

It's not great that we can create a TestDb without a Program which will then panic in some cases; it'd be ideal if creating a TestDb always created a fully functional db, or if the lack of a Program were handled more gracefully than a panic. Thanks for catching that! I can create an issue for it later; should be addressed separately. 

---

_Label `red-knot` added by @AlexWaygood on 2024-08-31 17:54_

---

_@dylwil3 reviewed on 2024-09-01 02:39_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/types/builder.rs`:278 on 2024-09-01 02:39_

Done!

---

_@dylwil3 reviewed on 2024-09-01 02:41_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-01 02:41_

Whoops that was definitely my bad 😅 Let me know if the current commit is any closer!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:68 on 2024-09-01 05:38_

nice approach!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:75 on 2024-09-01 05:40_

I would just eliminate this check; if someone uses a typeshed without a `bool` type, then `Unknown` is the right type here. (I think there's a separate TODO we should address, which is that in most cases where we look up a type from builtins, we should be replacing `Unbound` with `Unknown` -- but this code here shouldn't be worrying about that detail.)
```suggestion
            self.elements.remove(&Type::BooleanLiteral(true));
            self.elements.remove(&Type::BooleanLiteral(false));
            self.elements.insert(bool_ty);
```

---

_@carljm approved on 2024-09-01 05:42_

Looks great!

---

_Merged by @carljm on 2024-09-01 05:57_

---

_Closed by @carljm on 2024-09-01 05:57_

---

_Branch deleted on 2024-09-01 12:23_

---

_@MichaReiser reviewed on 2024-09-02 07:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:68 on 2024-09-02 07:56_

It would be interesting to look at the generated code. `into` creates an `OrderSet` which in turn allocates. 

I prefer using `self.elements.contains(Type::BooleanLiteral(true) && self.elements.contains(Type::BooleanLiteral(false))`, which does not depend on the compiler being able to see through that the allocation isn't necessary (which requires rather involved analysis)

---

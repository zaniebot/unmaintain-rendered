```yaml
number: 16415
title: "[red-knot] Don't use separate ID types for each alist"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/alist-type
created_at: 2025-02-27T14:52:12Z
updated_at: 2025-02-28T19:55:56Z
url: https://github.com/astral-sh/ruff/pull/16415
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Don't use separate ID types for each alist

---

_Pull request opened by @dcreager on 2025-02-27 14:52_

Regardless of whether #16408 and #16311 pan out, this part is worth pulling out as a separate PR.

Before, you had to define a new `IndexVec` index type for each type of association list you wanted to create.  Now there's a single index type that's internal to the alist implementation, and you use `List<K, V>` to store a handle to a particular list.

This also adds some property tests for the alist implementation.

---

_Label `internal` added by @dcreager on 2025-02-27 14:52_

---

_Label `red-knot` added by @dcreager on 2025-02-27 14:52_

---

_Review requested from @carljm by @dcreager on 2025-02-27 14:52_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-27 14:52_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-27 14:52_

---

_Review requested from @sharkdp by @dcreager on 2025-02-27 14:52_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:133 on 2025-02-27 14:54_

`List` is `Copy`, so I'm not sure why clippy want us to always pass this around by value like we were before.  I guess the extra phantom fields increase the perceived complexity?

---

_@dcreager reviewed on 2025-02-27 14:55_

---

_Comment by @github-actions[bot] on 2025-02-27 15:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:84 on 2025-02-27 15:08_

You can combine the two if you want. I also remember that derive macros sometimes struggle when using `PhantomData<T>` directly but using a fn that returns the types work fine.

```suggestion
    _type: PhantomData<() -> (K, V)>,
```

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:114 on 2025-02-27 15:10_

Removing the `I` makes this type less a clear fit for the `ruff_index` crate because it's no longer an indexed type. 

What's the benefit of removing the `I` type other than just removing the need to define an index newtype wrapper type? Didn't it protect that you could pass a list for another `ListStorage` with the same `K`, `V` to a `ListStorage` for another index?

---

_@MichaReiser reviewed on 2025-02-27 15:11_

It's not entirely clear to me what the benefit of removing the `Index` type is. Didn't it give us some added type safety that prevented mixing different list types?

Edit: It's very likely that I simply don't understand the data structure enough to see the benefit of it. In that case, feel free to disregard this comment.

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:84 on 2025-02-27 15:34_

Good idea! Done

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:114 on 2025-02-27 15:36_

> What's the benefit of removing the `I` type other than just removing the need to define an index newtype wrapper type? Didn't it protect that you could pass a list for another `ListStorage` with the same `K`, `V` to a `ListStorage` for another index?

We weren't (and as far as I know, aren't planning on) creating any lists with the same `K, V` but different `I` types.  So I was viewing this as removing a redundancy.

> Removing the `I` makes this type less a clear fit for the `ruff_index` crate because it's no longer an indexed type.

That's fair — would you want me to move this into a different crate if we keep the change?  Is there an existing crate that's a better fit, or would it deserve its own?

---

_@dcreager reviewed on 2025-02-27 15:37_

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:17 on 2025-02-27 15:37_

This seems to describe the previous API?

---

_@carljm reviewed on 2025-02-27 15:40_

---

_@MichaReiser reviewed on 2025-02-27 15:41_

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:114 on 2025-02-27 15:41_

> We weren't (and as far as I know, aren't planning on) creating any lists with the same K, V but different I types. So I was viewing this as removing a redundancy.

If there's value in having the separate index type than I think I'd prefer to keep it and instead have type aliases in `red_knot_python_semantic` or to move the types into `red_knot_python_semantic` if we don't intent to use it elsewhere. 

> That's fair — would you want me to move this into a different crate if we keep the change? Is there an existing crate that's a better fit, or would it deserve its own?

That's a tough question. I don't think there's a fitting crate for it but creating its own crate also feels somewhat overkill. I'm okay leaving it here or I'd move it into `red_knot_python_semantic`, considering it's the only place where we use the type.

---

_@dcreager reviewed on 2025-02-27 17:02_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:17 on 2025-02-27 17:02_

Yes, good catch, fixed!

---

_Comment by @carljm on 2025-02-28 01:00_

Discussed this briefly with @dcreager  in our 1:1. I don't think this really loses any type safety that we had previously. If you want to create two list arenas with the same `K` and `V` but whose lists are treated as different types, you can do that in the same way you would in the same scenario with any other collection type: wrap it in a newtype. This seems more ergonomic, more idiomatic, and just as type-safe as being forced to define a distinct newtype-index.

The most likely mistake in our use of these lists is that we would mistakenly use a list from one scope's list arena with the arena from a different scope. But these arenas are all the same type (and use the same newtype-index today), so we already don't have protection from this. (It's no different than any of our per-scope IndexVec newtype indices, which are invalid if used across scopes, and we don't have compiler-level protection from this.)

---

_@carljm approved on 2025-02-28 01:01_

This looks fine to me. No opposition to moving it into `red_knot_python_semantic`, at least until an external use case shows up.

---

_Comment by @dcreager on 2025-02-28 14:09_

> No opposition to moving it into `red_knot_python_semantic`, at least until an external use case shows up.

Done.  That required removing some methods that aren't currently being used (union, list iterator), but we can always resurrect those from the git history if needed.

---

_Merged by @dcreager on 2025-02-28 19:55_

---

_Closed by @dcreager on 2025-02-28 19:55_

---

_Branch deleted on 2025-02-28 19:55_

---

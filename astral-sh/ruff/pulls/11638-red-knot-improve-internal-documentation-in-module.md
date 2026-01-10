```yaml
number: 11638
title: "red-knot: improve internal documentation in `module.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: module-resolver
created_at: 2024-05-31T14:08:49Z
updated_at: 2024-05-31T19:19:37Z
url: https://github.com/astral-sh/ruff/pull/11638
synced_at: 2026-01-10T21:56:00Z
```

# red-knot: improve internal documentation in `module.rs`

---

_Pull request opened by @AlexWaygood on 2024-05-31 14:08_

This PR improves the internal documentation for various methods and structs in `module.rs`. The main purpose of this is to make sure I properly understand everything that the current code is doing :-)

---

_Review requested from @carljm by @AlexWaygood on 2024-05-31 14:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-31 14:08_

---

_Label `internal` added by @AlexWaygood on 2024-05-31 14:10_

---

_Comment by @github-actions[bot] on 2024-05-31 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:19 on 2024-05-31 14:47_

Another reason is that it is a specific concept that is worth its own name and it also stores additional data that isn't available on a path (except if you mean `ModulePath`)

---

_@MichaReiser approved on 2024-05-31 14:48_

Thanks. I'll have to copy those comments over. 

I also renamed a couple of things in my PR (`root` => `search_path`) and move them around but I can deal with this when it comes to merging my PR.

---

_@AlexWaygood reviewed on 2024-05-31 14:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:19 on 2024-05-31 14:57_

Right. At first I thought this struct was really a `ModuleID` struct because it seemed to be a thin wrapper around a primitive type. But now I see that the primitive type really is the ID for the module, and that the struct is correctly named. Which means that this comment is not quite accurate!

---

_@AlexWaygood reviewed on 2024-05-31 16:06_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:19 on 2024-05-31 16:06_

Hmm, but that isn't quite right either, I see? Since the `modules` field on the `ModuleResolver` struct maps `Module` objects to `ModuleData` objects, and its the `ModuleData` objects that seem to hold the interesting information about the module.

In general I feel slightly confused about whether this is meant to represent:
- a struct that can be queried directly to obtain interesting information about a module (this is what would be implied by the name `Module` to me); or,
- a cheap ID that can be used to easily retrieve some other object that can be queried directly to obtain interesting information about a module (similar to `FileID` elsewhere in redknot, or to `BindingID` in `ruff_python_semantic/binding.rs`).

Currently it feels like it maybe sits awkwardly between the two concepts

---

_@AlexWaygood reviewed on 2024-05-31 16:07_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:19 on 2024-05-31 16:07_

I think I'll just remove this comment and try to refactor this into something I like more ;)

---

_Merged by @AlexWaygood on 2024-05-31 16:11_

---

_Closed by @AlexWaygood on 2024-05-31 16:11_

---

_Branch deleted on 2024-05-31 16:11_

---

_Comment by @carljm on 2024-05-31 17:56_

This is excellent, thank you!!

---

_@MichaReiser reviewed on 2024-05-31 18:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:19 on 2024-05-31 18:52_

The way module is defined is very intentional. Not necessarily the fields it stores and its name but that it is a thin wrapper around a `u32` with methods to query its fields. From an API perspective, this is the `Module` and an outside client doesn't need to be concerned about how it stores the data internally. 

The reason why we shouldn't change this representation significantly is because this exactly maps to Salsa. You can have a look at my PR and you'll find something very similar: 

* `ResolvedModule` is a newtype wrapper around a `u32`. This is important for fast cache lookups and to avoid awkward lifetimes. It also ensures that we can store a `ResolvedModule` very cheaply, it's just 4 bytes.
* It has accessor functions that take `db` as an argument and return the field data

We can't change this without breaking Salsa compatibility. Ideally, you try to keep the API roughly unchanged (or use something similar to the salsa PR). But feel free to restructure the internal data structures however you want.


---

_@AlexWaygood reviewed on 2024-05-31 19:19_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:19 on 2024-05-31 19:19_

Thanks. I was confused by the names of variables such as this in `module.rs`, which seemed to imply to me that `Module` instances did not in fact represent modules -- that they only represented module IDs that could be used to lookup modules, and that it was in fact `ModuleData` instances that represented modules. Here the `id` variable has type `Module`, and the `module` variable has type `ModuleData`:

https://github.com/astral-sh/ruff/blob/7ce17b773643556d9d2c7aab1506ece87991c0f0/crates/red_knot/src/module.rs#L491-L500

I'll create another PR to try to clarify things further in the internal documentation.

---

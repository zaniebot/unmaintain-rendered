```yaml
number: 18898
title: "[ty] Include imported sub-modules as attributes on modules for completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/import-submodule
created_at: 2025-06-23T15:54:49Z
updated_at: 2025-06-23T16:48:17Z
url: https://github.com/astral-sh/ruff/pull/18898
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Include imported sub-modules as attributes on modules for completions

---

_Pull request opened by @BurntSushi on 2025-06-23 15:54_

This also adds a new `ModuleName::relative_to` public API to help with
this.

Kudos to @AlexWaygood for the meat of this patch!

Ref https://github.com/astral-sh/ruff/pull/18830#discussion_r2161770991


---

_Review requested from @carljm by @BurntSushi on 2025-06-23 15:54_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-23 15:54_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-23 15:54_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-23 15:54_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-23 15:54_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-23 15:55_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-23 15:55_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-06-23 15:55_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-23 15:55_

---

_Label `ty` added by @AlexWaygood on 2025-06-23 15:56_

---

_Comment by @github-actions[bot] on 2025-06-23 15:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_name.rs`:171 on 2025-06-23 15:58_

Nit: Can't we use `self.0.strip_prefix(&*parent.0)` directly here or why do we need to check for `starts_with` firstr?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_name.rs`:173 on 2025-06-23 15:58_

Should this return `None` instead of panicking?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_name.rs`:174 on 2025-06-23 16:02_

Should/could we use this in https://github.com/astral-sh/ruff/blob/c9dff5c7d5bcab2a83f1c3b4a1a80af230ece541/crates/ty_python_semantic/src/module_name.rs#L291-L295

---

_@MichaReiser approved on 2025-06-23 16:02_

---

_Label `server` added by @MichaReiser on 2025-06-23 16:02_

---

_@AlexWaygood approved on 2025-06-23 16:03_

Nice! 😃

---

_@AlexWaygood reviewed on 2025-06-23 16:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:171 on 2025-06-23 16:06_

> why do we need to check for `starts_with` firstr?

Just using `strip_prefix` would give inaccurate results because of the fact that the underlying data is a string rather than a sequence of strings. `ModuleName("importlibbbbb.resources").starts_with(ModuleName("importlib"))` returns `false` (which follows Python's semantics correctly) but `"importlibbbbb.resources".strip_prefix("importlib")` returns `Some()`.

---

_@AlexWaygood reviewed on 2025-06-23 16:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:173 on 2025-06-23 16:12_

It's a logic error to end up with an empty string here:
- If `self` is not a relative `ModuleName` to `parent`, `self.0.strip_prefix(&*parent.0)` will return `None`
- If `self` is the same `ModuleName` as `parent`, `self.0.strip_prefix(&*parent.0)` will return `Some("")` and the second `strip_prefix()` call will return `None`

I don't think there's any way in which we'd get an empty string here if both `self` and `parent` are valid `ModuleName`s (which should be guaranteed by the invariants upheld by the `ModuleName` constructor methods)

---

_@AlexWaygood reviewed on 2025-06-23 16:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:174 on 2025-06-23 16:14_

I think that function is doing something different. A better name for that function might be `module_name_from_relative_import`

---

_@BurntSushi reviewed on 2025-06-23 16:25_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:171 on 2025-06-23 16:25_

I think Micha is right here. While that first `strip_prefix` call would return `Some`, the subsequent `strip_prefix('.')` call will prevent your example from returning `Some` overall. Since iterating over the components of a module name is just a simple splitting on `.`, this seems correct to me.

---

_@BurntSushi reviewed on 2025-06-23 16:25_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:173 on 2025-06-23 16:25_

I agree. I distilled Alex's reasoning into a small comment on the assert.

---

_@BurntSushi reviewed on 2025-06-23 16:26_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:174 on 2025-06-23 16:26_

Aside from using `relative_module_name` directly and taking this suggestion more holistically, I don't see a simpler way of utilizing `ModuleName::ancestors` (or `ModuleName::components`) to get what we want here.

---

_@AlexWaygood reviewed on 2025-06-23 16:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:171 on 2025-06-23 16:31_

oh of course, right!

---

_@BurntSushi reviewed on 2025-06-23 16:31_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:171 on 2025-06-23 16:31_

(Sorry, I just ninja-edited my previous comment. I had written one part backwards. I've updated this PR to get rid of the initial `starts_with` call.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:180 on 2025-06-23 16:33_

you could maybe also add a debug assertion encapsulating your explanation in https://github.com/astral-sh/ruff/pull/18898#discussion_r2162012481 of why it's correct to just rely on the `strip_prefix()` calls here:

```suggestion
        // At this point, `relative_name` *has* to be a
        // proper suffix of `self`. Otherwise, one of the two
        // `strip_prefix` calls above would return `None`.
        // (Notably, a valid `ModuleName` cannot end with a `.`.)
        assert!(!relative_name.is_empty());
        debug_assert!(self.starts_with(parent));
```

---

_@AlexWaygood approved on 2025-06-23 16:33_

---

_@AlexWaygood reviewed on 2025-06-23 16:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:174 on 2025-06-23 16:36_

I think Micha was suggesting the opposite: that we could make use of the new method you're adding in the existing `relative_module_name` function (which, confusingly, actually constructs an absolute `ModuleName` from a relative import statement -- close to the opposite to what we're doing in this new method, where we take an absolute `ModuleName` and try to turn it into a relative one)

---

_@BurntSushi reviewed on 2025-06-23 16:38_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:180 on 2025-06-23 16:38_

Good point. Done. I thought this was a little paranoid, and I was inclined to resist since `strip_prefix` seems to make this vacuously true... but that's wrong. Because `starts_with` is smarter than a simple string prefix check. In theory, the implementations could diverge such that this assert does indeed trip.

---

_@BurntSushi reviewed on 2025-06-23 16:45_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_name.rs`:174 on 2025-06-23 16:45_

Oh. Derp. Yes.

---

_Merged by @BurntSushi on 2025-06-23 16:48_

---

_Closed by @BurntSushi on 2025-06-23 16:48_

---

_Branch deleted on 2025-06-23 16:48_

---

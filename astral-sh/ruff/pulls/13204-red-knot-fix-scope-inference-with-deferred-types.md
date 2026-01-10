```yaml
number: 13204
title: "[red-knot] fix scope inference with deferred types"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/deferred-panic
created_at: 2024-09-02T02:14:12Z
updated_at: 2024-09-03T18:20:45Z
url: https://github.com/astral-sh/ruff/pull/13204
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] fix scope inference with deferred types

---

_Pull request opened by @carljm on 2024-09-02 02:14_

Test coverage for #13131 wasn't as good as I thought it was, because although we infer a lot of types in stubs in typeshed, we don't check typeshed, and therefore we don't do scope-level inference and pull all types for a scope. So we didn't really have good test coverage for scope-level inference in a stub. And because of this, I got the code for supporting that wrong, meaning that if we did scope-level inference with deferred types, we'd end up never populating the deferred types in the scope's `TypeInference`, which causes panics like #13160.

Here I both add test coverage by running the corpus tests both as `.py` and as `.pyi` (which reveals the panic), and I fix the code to support deferred types in scope inference.

This also revealed a problem with deferred types in generic functions, which effectively span two scopes. That problem will require a bit more thought, and I don't want to block this PR on it, so for now I just don't defer annotations on generic functions.

Fixes #13160.


---

_Label `red-knot` added by @carljm on 2024-09-02 02:14_

---

_Review requested from @MichaReiser by @carljm on 2024-09-02 02:14_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-02 02:14_

---

_Comment by @github-actions[bot] on 2024-09-02 02:38_

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

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:46 on 2024-09-02 10:53_

`SystemPath` has a `as_std_path()` method. You may be able to avoid some path -> sys path conversion by using it.

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:27 on 2024-09-02 10:57_

I see the value in extending all corpus tests to `pyi` files and it kind of works but I could see this break in the future because the `stub_dir` isn't part of the workspace. This might cause problems in the future when we try to resolve settings because we won't have any settings available for files outside the workspace. 

I don't have a good suggestion on how to solve this differently other than copying a handful of tests and manually rename the files to `.pyi` files. 

---

_@MichaReiser approved on 2024-09-02 10:58_

---

_@carljm reviewed on 2024-09-03 13:28_

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:27 on 2024-09-03 13:28_

I assume we will always need to support someone opening an out-of-workspace file? We just won't be able to resolve relative imports from such files. Which shouldn't be an issue for corpus tests. It might actually be useful to have a check that this doesn't break?

But if you'd rather, I can change the test so that it copies all the `.py` files to the tempdir also, and sets the tempdir as the workspace root?

---

_@carljm reviewed on 2024-09-03 13:30_

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:46 on 2024-09-03 13:30_

I don't think I'm currently doing any more than the minimum needed path -> sys path conversions -- I have a std Path, and I convert it to a SystemPath, but I keep the original std Path around as well. I guess I could not keep the original std Path around, and instead use `as_std_path` to convert back? But that's doing more conversions, not less. It just allows using fewer variable names (I wouldn't need both `path` and `syspath`, I could shadow instead.)

---

_@MichaReiser reviewed on 2024-09-03 13:36_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:27 on 2024-09-03 13:36_

> I assume we will always need to support someone opening an out-of-workspace file? 

Yes, but it still needs to be clarified how we want to support this in the editor. One option is to create an ad-hoc workspace for all detached files. Another option is to list the detached files in the workspace explicitly. The issue with just using the workspace is that any detached file would automatically inherit the workspace setting. That might or might not be what we want. I don't see this use case in the CLI. 


> But if you'd rather, I can change the test so that it copies all the .py files to the tempdir also, and sets the tempdir as the workspace root?

That's a good idea. I would prefer this. 

---

_Merged by @carljm on 2024-09-03 18:20_

---

_Closed by @carljm on 2024-09-03 18:20_

---

_Branch deleted on 2024-09-03 18:20_

---

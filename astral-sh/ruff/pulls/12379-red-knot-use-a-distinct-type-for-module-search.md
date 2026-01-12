```yaml
number: 12379
title: "[red-knot] Use a distinct type for module search paths in the module resolver"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: search-path-consistent-type-2
created_at: 2024-07-18T13:49:04Z
updated_at: 2024-07-22T19:53:48Z
url: https://github.com/astral-sh/ruff/pull/12379
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Use a distinct type for module search paths in the module resolver

---

_@AlexWaygood_

## Summary

This PR cleans up some of the settings validation and types in the module resolver. Namely, at various points in the crate, we use a type alias, a ModuleResolutionPathBuf or an `Arc<ModuleResolutionPathBuf>` to refer to a module search path. This PR goes some way to cleaning that up by introducing a newtype wrapper in path.rs for module-resolution paths that specifically represent search paths.

The main motivation for this change is because module search paths and other module-resolution paths have different invariants, so should be validated differently. This PR has been split off from https://github.com/astral-sh/ruff/pull/12376, which bundled the validation changes in as the same PR, to ease review.

Copying from the PR description of #12376:

> This PR goes halfway to addressing @carljm's review comments in https://github.com/astral-sh/ruff/pull/12141#discussion_r1667010245. We now have a distinct type for representing search paths specifically outside of `path.rs`. However, internally, `path.rs` still doesn't make much distinction between search paths and other kinds of paths: it doesn't have to, because I implemented `Deref` for the new `ModuleSearchPath` struct; all methods available on `ModuleResolutionPathBuf` are implicitly available on `ModuleSearchPath`. This is somewhat lazy and unprincipled of me, and I've left a TODO comment to get rid of it. I spent much of yesterday trying to get to a more principled solution, but it ends up being a massive diff that rewrites much of `path.rs`, and I don't think it's a priority for now. I think this is a nice cleanup that clarifies the types and improves settings validation in the meantime, without being a massive rewrite that would be time-consuming to write and review.

## Test Plan

`cargo test -p red_knot_module_resolver`


---

_Label `red-knot` added by @AlexWaygood on 2024-07-18 13:49_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-18 13:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-18 13:49_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:752 on 2024-07-18 14:00_

I think we should inline the 

https://github.com/astral-sh/ruff/blob/dc8ae368ff6a5dedd7e623393f700c8709362cb6/crates/red_knot_module_resolver/src/path.rs#L171-L212

and remove the definitions from `ModuleResolutionPathBuf`. The methods on `ModuleResolutionPathBuf` are only used in tests and I suspect that the tests should probably use `ModuleResolutionPath` instead.

---

_Comment by @github-actions[bot] on 2024-07-18 14:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:729 on 2024-07-18 14:02_

The naming is kind of confusing now with `ModuleSearchPath`, `ModuleSearchPathBuf` and `ModuleSearchPathRef`. 

Can we rename the `PathBuf` and `PathRef` structs to maybe `ModulePathBuf` and `ModulePathRef`. At least I understand that these are now paths to modules and not to the search paths. 

These structs could also use some documentation to explain the intent.


---

_@MichaReiser reviewed on 2024-07-18 14:06_

I let @carljm approve this change. I like that some `Arc<ModuleSearchPath>` are gone. I'm otherwise not quiet sure what the benefit is. 

Nit: We can reduce the visibility of `ModuleSearchPathBuf` and `Ref` to `pub(super)`. 

---

_Comment by @AlexWaygood on 2024-07-18 14:33_

> I'm otherwise not quiet sure what the benefit is.

Yeah, it's harder to see the benefit when the validation changes are decoupled from the change to use a new type. This is partly why I originally put them together in one PR, in https://github.com/astral-sh/ruff/pull/12376.

---

_@AlexWaygood reviewed on 2024-07-18 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:729 on 2024-07-18 14:35_

> The naming is kind of confusing now with `ModuleSearchPath`, `ModuleSearchPathBuf` and `ModuleSearchPathRef`.

Those aren't the names I've used -- the three structs are `ModuleSearchPath`, `ModuleResolutionPathBuf` and `ModuleResolutionPathRef`, which isn't _so_ confusing. But I like your proposed rename.

> These structs could also use some documentation to explain the intent.

Sure

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:729 on 2024-07-20 00:44_

I also think the word "Resolution" doesn't add value, and would prefer the names Micha suggested.

---

_@carljm approved on 2024-07-20 00:49_

This looks like a good start that brings a bit of extra naming clarity, but doesn't do the real refactor yet, just as advertised.

I agree with both of Micha's comments.

I'm fine landing this and holding off on any more refactor for now, until/unless there are actual problems caused by the current structure.

---

_@AlexWaygood reviewed on 2024-07-22 19:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:752 on 2024-07-22 19:40_

This makes sense but I'm going to postpone it for now. The idea of this PR was that it would be a small, incremental change that would be easy to review. Addressing this would require lots of changes to tests, and I think this PR is an improvement over the status quo as it stands.

---

_Renamed from "[red-knot] Use a distinct type for module search paths in the module resolver (v2)" to "[red-knot] Use a distinct type for module search paths in the module resolver" by @AlexWaygood on 2024-07-22 19:40_

---

_Merged by @AlexWaygood on 2024-07-22 19:44_

---

_Closed by @AlexWaygood on 2024-07-22 19:44_

---

_Branch deleted on 2024-07-22 19:44_

---

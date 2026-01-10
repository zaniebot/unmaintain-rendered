```yaml
number: 13055
title: "[red-knot] Improve the accuracy of the unresolved-import check"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-import-2
created_at: 2024-08-22T12:34:47Z
updated_at: 2024-08-27T13:17:24Z
url: https://github.com/astral-sh/ruff/pull/13055
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Improve the accuracy of the unresolved-import check

---

_Pull request opened by @AlexWaygood on 2024-08-22 12:34_

## Summary

This PR attempts to address @carljm's post-merge review of #13007:
- Reduce false positives from the check by emitting the diagnostic before transforming the `Unbound` type into `Unknown`. I'm [still unsure](https://github.com/astral-sh/ruff/pull/13007#discussion_r1726880479) that this fixes all problems, but I agree that it's definitely an improvement on the status quo.
- Get rid of the `ModuleResolutionEnum`
- Use more active voice in error messages

## Test Plan

- `cargo test -p red_knot_python_semantic --lib`
- `cargo codspeed build --features codspeed -p ruff_benchmark && cargo codspeed run`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-22 12:34_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-22 12:34_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-22 12:34_

---

_@AlexWaygood reviewed on 2024-08-22 12:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:986 on 2024-08-22 12:37_

Several things feel like they become more awkward about this function as a result of getting rid of the `ModuleResolutionError` enum, FWIW:
- It now has to take `&mut self` rather than `&self`, since we emit diagnostics directly from this method
- It has to be passed the `import_from` node as an additional argument so that it can emit diagnostics directly. This is annoying because the `import_from` node has its own `.level` field, but we don't want that to be used from this method because we only call this method if we've already determined that `level > 0`, which is enforced by the fact that the type signature takes a `NonZeroU32`.
- Because the diagnostic could be emitted in several places from this function, I had to factor it out into a separate helper function

---

_Comment by @AlexWaygood on 2024-08-22 12:52_

In case it isn't clear: my goal with the PRs relating to this check isn't necessarily to create a "perfect unresolved-import" check; I agree that that isn't really a priority right now. What I'm interested in is whether our current infrastructure propagates enough information for us to be able to emit precise diagnostics for end users, that have helpful error messages -- because if not, then that could require more significant refactors to our architecture (which I think we'd rather know about sooner rather than later).

---

_Comment by @github-actions[bot] on 2024-08-22 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:986 on 2024-08-22 19:44_

I don't find the first or third bullet bothersome. The second I agree is irritating. It seems like one way to make things cleaner would be to make the signature just `fn import_from_module_name(&mut self, import_from: &ast::StmtImportFrom) -> Option<ModuleName>` and always call it (i.e. incorporate the outer `match` from the callsite into it.) This method only has one call site so it's pretty much arbitrary where we draw the boundary around it.

If you strongly prefer using an error enum, I'm open to sticking with that; I just find it generally simpler and clearer to emit diagnostics where they occur and avoid temporary extra representations.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1129 on 2024-08-22 21:06_

I'm happy with this error message; if you prefer hewing closer to the runtime error and going with `Cannot import '{}' from module '{}'`, I'm fine with that, too.

---

_@carljm approved on 2024-08-22 21:07_

Looks good!

---

_@AlexWaygood reviewed on 2024-08-27 10:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1129 on 2024-08-27 10:34_

I actually quite like this message. I think it's specifically the term "attribute" that I'd like to avoid here -- although it's accurate, it's also confusing IMO, because people don't _think_ of `from` imports as attribute accesses (even though they are), and because we'll likely have an `attribute-access` or `unknown-attribute` error code that's separate to the `unresolved-import` error code. Referring to a "member" rather than an "attribute" sidesteps this without being less precise (in my opinion), and is also slightly more specific than the runtime error message about _why_ the import failed.

---

_@AlexWaygood reviewed on 2024-08-27 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:986 on 2024-08-27 11:30_

> It seems like one way to make things cleaner would be to make the signature just `fn import_from_module_name(&mut self, import_from: &ast::StmtImportFrom) -> Option<ModuleName>` and always call it (i.e. incorporate the outer `match` from the callsite into it.) This method only has one call site so it's pretty much arbitrary where we draw the boundary around it.

I just tried that locally. It _works_... but it still feels messy, however, in my opinion, because it's hard to keep track of where we're emitting tracing logs to note invalid syntax. Since the various errors are handled inside `relative_module_name()`, it makese sense to do the logging inside that method. But the caller of the method has to handle the fact that the object returned from `relative_module_name()` could be `None`, and it feels very strange to discard the `None` and replace it with `Type::Unknown` at the callsite without doing any logging _there_. So you end up having to add a verbose comment to the callsite explaining that you've already done the logging inside the helper function -- at which point, you really start to wonder if this is the best design, or if it wouldn't be better for the helper function to be stateless and propagate errors to the caller for the caller to log tracing messages about 😄

> If you strongly prefer using an error enum, I'm open to sticking with that; I just find it generally simpler and clearer to emit diagnostics where they occur and avoid temporary extra representations.

In general I absolutely agree... but here, it feels like there's just too many branches

---

_Merged by @AlexWaygood on 2024-08-27 13:17_

---

_Closed by @AlexWaygood on 2024-08-27 13:17_

---

_Branch deleted on 2024-08-27 13:17_

---

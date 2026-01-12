```yaml
number: 19605
title: "[ty] Implement diagnostic caching"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/cache-workspace-diagnostics
created_at: 2025-07-28T19:55:22Z
updated_at: 2025-07-30T10:08:34Z
url: https://github.com/astral-sh/ruff/pull/19605
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Implement diagnostic caching

---

_@MichaReiser_

## Summary

This PR adds support for caching workspace diagnostics. 

For each file, ty now sends a `resultId` that is the hash of all diagnostics for that file. The client sends the `resultId`s from the previous workspace diagnostic requests as part of the next request payload. This allows ty to determine if the diagnostics of a file changed and only send a full diagnostic report for those files (and send unchanged for all other files).

I hoped that this would fix the observed lag for home assistant when workspace diagnostics are enabled. Unfortunately, this isn't the case. It reduces the lag, but it doesn't fix it entirely 

## Test Plan


https://github.com/user-attachments/assets/3b0d1a3d-21cd-49a3-8c43-7b2781b1c02f




---

_Label `server` added by @MichaReiser on 2025-07-28 19:55_

---

_Label `ty` added by @MichaReiser on 2025-07-28 19:55_

---

_Label `server` added by @MichaReiser on 2025-07-28 19:55_

---

_Label `ty` added by @MichaReiser on 2025-07-28 19:55_

---

_Comment by @github-actions[bot] on 2025-07-28 19:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-07-30 09:58:15.659765386 +0000
+++ new-output.txt	2025-07-30 09:58:15.723765355 +0000
@@ -87,6 +87,7 @@
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[T_co, T_contra]'>` with no `__class_getitem__` method
+annotations_coroutines.py:27:5: error[type-assertion-failure] Argument does not have asserted type `str`
 annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -101,6 +102,8 @@
 annotations_forward_refs.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
 annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`
 annotations_generators.py:91:27: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.AsyncGeneratorType`
+annotations_generators.py:167:5: error[type-assertion-failure] Argument does not have asserted type `AsyncGenerator[str, None]`
+annotations_generators.py:174:5: error[type-assertion-failure] Argument does not have asserted type `AsyncGenerator[str, None]`
 annotations_generators.py:193:1: error[type-assertion-failure] Argument does not have asserted type `() -> AsyncIterator[int]`
 annotations_methods.py:31:1: error[type-assertion-failure] Argument does not have asserted type `A`
 annotations_methods.py:36:1: error[type-assertion-failure] Argument does not have asserted type `B`
@@ -889,4 +892,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 890 diagnostics
+Found 893 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-28 19:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-28 20:07_

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

_Review requested from @dhruvmanila by @MichaReiser on 2025-07-29 13:29_

---

_Marked ready for review by @MichaReiser on 2025-07-29 14:35_

---

_Review requested from @carljm by @MichaReiser on 2025-07-29 14:35_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-29 14:35_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-29 14:35_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-29 14:35_

---

_Review request for @dcreager removed by @MichaReiser on 2025-07-29 14:35_

---

_Review request for @carljm removed by @MichaReiser on 2025-07-29 14:35_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-07-29 14:35_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-07-29 14:35_

---

_Comment by @dhruvmanila on 2025-07-29 16:24_

(I plan to review this first thing tomorrow morning)

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:38 on 2025-07-30 03:16_

nit: we could probably remove this comment now?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:117 on 2025-07-30 03:24_

nit: we could probably consume `previous_results` (via `.into_iter()`) to avoid the clone on `previous_url`

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:39 on 2025-07-30 03:26_

Should this be applied globally in `TextContext::new`?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:222 on 2025-07-30 03:35_

I wonder if this should actually be fixed in the test server instead to mimic what a client would do. This is because, I think, it's VS Code that does this part or more specifically the ty VS Code extension that creates the workspace and global settings and sends it via the initialization options.

The extension would construct workspace settings by asking VS Code the values for each setting which would automatically fallback to the global value. This means the server doesn't need to do any resolution between workspace and global values. This is what Ruff (and ty) does currently.

https://github.com/astral-sh/ty-vscode/blob/641781c780fad4d575c3ee715e2a6f70e5e29a8c/src/common/settings.ts

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:233 on 2025-07-30 03:36_

Is this required for workspace diagnostics to work?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:237 on 2025-07-30 03:36_

A follow-up (good help wanted task) would be to render these diagnostics similar to the ones in server unit tests

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:120 on 2025-07-30 03:37_

Should we `panic!` here similar to `WorkspaceDiagnosticReportResult::Partial` branch?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:134 on 2025-07-30 03:39_

Should we close these files after making these changes? We could have a helper method on `TestServer` that does this for us but it also isn't too verbose here either.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:317 on 2025-07-30 03:40_

Should we add a test case where the diagnostic content didn't change but the range did? Like, new content got added to the file but the new content doesn't produce the diagnostics so the existing diagnostic just got moved further down.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:767 on 2025-07-30 03:41_

I think we should use `initialization_options` instead of `global_options` which is what this is from a client's perspective.

---

_@dhruvmanila approved on 2025-07-30 03:43_

Thank you!

This looks relatively straightforward. Do you know how much of a performance improvement does this make?

---

_Comment by @dhruvmanila on 2025-07-30 03:48_

Another follow-up would be to add this to document diagnostic as well (`textDocument/diagnostic`) but I'm not sure if it's beneficial in that request because that request would be sent every time the file got changed and whenever a user is typing, the change would usually produce a syntax error initially before the code gets in a stable state. One scenario where it would be useful is when copy pasting a code snippet which doesn't produce any diagnostic and the locations of existing diagnostics didn't change.

---

_Comment by @MichaReiser on 2025-07-30 06:38_

> This looks relatively straightforward. Do you know how much of a performance improvement does this make?

I don't know how I'd measure this. I'd need to plug into VS code's rendering performance (it's re-rendering the problems list that is slow)

---

_@MichaReiser reviewed on 2025-07-30 06:39_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:39 on 2025-07-30 06:39_

I don't think we should because the filter isn't very specific. It replaces all 16 digit numbers with `RESULT_ID`. I'd be worried that it replaces some other random numbers.

---

_@MichaReiser reviewed on 2025-07-30 06:43_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:222 on 2025-07-30 06:43_

Ideally, we'd fix this in the server itself, but it feels out of scope for this PR. I'm not entirely sure how I would model this but the server should prefer the option specified in the global option over a value provided in the workspace configuration. In fact, it should only use the workspace level option if it wasn't provided globally. But I'd prefer not to make changes to settings while you're also doing a refactor. That's why I'd prefer to leave this as is for now

---

_@MichaReiser reviewed on 2025-07-30 06:43_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:233 on 2025-07-30 06:43_

It's not. It just feels more realistic to have at least one file open :)

---

_@MichaReiser reviewed on 2025-07-30 06:45_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:237 on 2025-07-30 06:45_

I'm not sure if this is worth it here, given that we don't really care about the diagnostic content. The only part we're interested in is if they're full results or unchanged. I think I rather redact the diagnostic content in snapshots than render them in a fancy way to make the test less prone to changes in ty's core.

---

_@MichaReiser reviewed on 2025-07-30 06:47_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:134 on 2025-07-30 06:47_

I think that makes sense for a test, testing the workspace diagnostic mode. I don't think it's necessary here and it only complicates the test (it's also sort of tested already by the fact that the initial run only opens 1 file)



---

_@MichaReiser reviewed on 2025-07-30 06:49_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:767 on 2025-07-30 06:49_

Would that require that the test specify the entire `initialization_options`? Like, with `{ "settings": global_options }`? 


I'd prefer if we can avoid this. It took me longer than it should have to figure out the right structure of our `initialization_options`. 

---

_@dhruvmanila reviewed on 2025-07-30 08:01_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:237 on 2025-07-30 08:01_

Ah yeah, I think that makes sense.

---

_@dhruvmanila reviewed on 2025-07-30 08:04_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:767 on 2025-07-30 08:04_

> Would that require that the test specify the entire `initialization_options`? Like, with `{ "settings": global_options }`?

No, I just want to indicate that these are the initialization options because I'd find global options a bit confusing from a client perspective.

---

_Comment by @dhruvmanila on 2025-07-30 08:04_

> I don't know how I'd measure this. I'd need to plug into VS code's rendering performance (it's re-rendering the problems list that is slow)

A quick way would be to get the numbers that the server logs? I think it logs how long it took a request at the debug or trace level.

---

_@dhruvmanila reviewed on 2025-07-30 08:06_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:222 on 2025-07-30 08:06_

That sounds reasonable.

---

_@MichaReiser reviewed on 2025-07-30 08:50_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:767 on 2025-07-30 08:50_

I can rename it and I just saw that this will take `InitializationOptions` in your settings pr

---

_@MichaReiser reviewed on 2025-07-30 08:51_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:38 on 2025-07-30 08:51_

Yes, while it is still true, we shouldn't hit this log as frequently anymore

---

_@MichaReiser reviewed on 2025-07-30 08:56_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:117 on 2025-07-30 08:56_

It already uses `into_iter` implicitly. The reason we need to clone the `url` is because `key` requires an owned `Url` and we also need the URL for the `FullDiagnosticReportItem`. It might be possible to avoid the URL clone by getting the `url` from the `DocumentRef` (adding an `into_url` method or similar) but it complicates the code a fair amount which I don't think is justified

---

_Comment by @MichaReiser on 2025-07-30 09:00_

> A quick way would be to get the numbers that the server logs? I think it logs how long it took a request at the debug or trace level.

I don't think this number will be very meaningful. The issue isn't that the request takes too long. It's that VS code dies diffing the diagnostics and rendering the new diagnostics

---

_Comment by @MichaReiser on 2025-07-30 10:00_

> Another follow-up would be to add this to document diagnostic as well (textDocument/diagnostic) but I'm not sure if it's beneficial in that request because that request would be sent every time the file got changed and whenever a user is typing, the change would usually produce a syntax error initially before the code gets in a stable state. One scenario where it would be useful is when copy pasting a code snippet which doesn't produce any diagnostic and the locations of existing diagnostics didn't change.

Done. I think it's still useful because an editor might send `pullDiagnostic` requests for multiple files and the diagnostics won't change for all of them

---

_Renamed from "[ty] Cache workspace diagnostics" to "[ty] Implement diagnostic caching" by @MichaReiser on 2025-07-30 10:00_

---

_Merged by @MichaReiser on 2025-07-30 10:04_

---

_Closed by @MichaReiser on 2025-07-30 10:04_

---

_Branch deleted on 2025-07-30 10:04_

---

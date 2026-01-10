```yaml
number: 19670
title: "[ty] Implement long-polling for workspace diagnsotics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/long-polling
created_at: 2025-07-31T19:10:26Z
updated_at: 2025-08-04T10:33:05Z
url: https://github.com/astral-sh/ruff/pull/19670
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implement long-polling for workspace diagnsotics

---

_Pull request opened by @MichaReiser on 2025-07-31 19:10_

## Summary

This PR implements long-polling for workspace diagnostics (as described in the spec).

Without long polling, VS Code keeps sending workspace/diagnostic requests every 2 seconds, even if the user didn't make any changes. This can take up a fair amount of CPU usage, even if all diagnostics are cached, because we still need to iterate over all files. 

This PR implements long polling which avoids this. The basic idea is that the server keeps the `workspace/diagnostic` open and only responds once there are changes (e.g. new or fixed diagnostics) or the server shuts down (or cancels the request). 

Implementing this required adding a `process` method to the request handler in order that workspace diagnostics can omit the response when long polling.

This PR implements the last optimization mentioned in https://github.com/astral-sh/ty/issues/824

Closes https://github.com/astral-sh/ty/issues/824

I can no longer reproduce https://github.com/astral-sh/ty/issues/818 on this PR (closes https://github.com/astral-sh/ty/issues/818)

## Test Plan

Added integration tests

https://github.com/user-attachments/assets/36c9d227-58c2-416b-9e29-a93a47c072b0


https://github.com/user-attachments/assets/18f03d21-8332-488d-ab34-dfc0f06e60aa




---

_Label `server` added by @MichaReiser on 2025-07-31 19:10_

---

_Label `ty` added by @MichaReiser on 2025-07-31 19:10_

---

_Comment by @github-actions[bot] on 2025-07-31 19:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 08:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-08-01 11:09_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:135 on 2025-08-01 11:09_

Whooops

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-01 15:44_

---

_Marked ready for review by @MichaReiser on 2025-08-01 15:44_

---

_Review requested from @carljm by @MichaReiser on 2025-08-01 15:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-01 15:44_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-01 15:44_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-01 15:44_

---

_Comment by @github-actions[bot] on 2025-08-01 15:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-01 16:11_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/traits.rs`:101 on 2025-08-04 06:07_

Can you please update the documentation for all the traits and the module level documentation as well? I think it'd be useful to specify the difference between "run" and "process"

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:350 on 2025-08-04 06:12_

```suggestion
        // If there's any pending workspace diagnostic long-polling request,
        // resume it if the session revision changed (e.g. because some document changed).
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:38 on 2025-08-04 06:18_

```suggestion
/// to send multiple responses (in the form of `$/progress` notifications) for
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:78 on 2025-08-04 06:19_

```suggestion
/// or all diagnostics are unchanged. Instead, the server keeps the request open (it doesn't reply)
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:83 on 2025-08-04 06:19_

```suggestion
/// the background thread running because holding on to the [`ProjectDatabase`] references
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:179 on 2025-08-04 06:21_

```suggestion
                // Don't respond, keep the request open (long polling).
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:154 on 2025-08-04 06:23_

What's a "Bull response" ?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:146 on 2025-08-04 06:24_

This comment doesn't seem relevant to what's happening with the code which simply calls `run`, is this intended to be placed somewhere else?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:32 on 2025-08-04 06:26_

nit: It might be useful to document when the return type will be `None` and `Some`

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:135 on 2025-08-04 06:27_

Oh lol, is it that the request were never retried before this PR then?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:216 on 2025-08-04 06:28_

I think it'd be useful to document some example cases when this should be bumped.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:203 on 2025-08-04 06:31_

nit: it might be worth adding a debug log for this "suspended request got cancelled" scenario

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:586 on 2025-08-04 06:33_

Why do we need to bump the version when a file is closed? Workspace diagnostics shouldn't change when a file is closed as they would still be visible in the diagnostics panel in an editor.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:626 on 2025-08-04 06:38_

Can we update the `workspace_diagnostic_request` method on `TestServer` with more parameters like the progress params, partial result params, etc. instead of adding another function here?

---

_@dhruvmanila approved on 2025-08-04 06:42_

This is awesome! Thank you for taking on the entire performance improvement part of workspace diagnostics!

---

_@MichaReiser reviewed on 2025-08-04 06:44_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:135 on 2025-08-04 06:44_

yes 😅 

---

_@MichaReiser reviewed on 2025-08-04 06:48_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:626 on 2025-08-04 06:48_

I sort of prefer test-local helpers because the method on `TestServer` otherwise has to accept a `WorkspaceDiagnosticParams` to support all combinations, in which case it is about as verbose as calling the method directly. What I can do is to give this function a better name

---

_@dhruvmanila reviewed on 2025-08-04 06:52_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:626 on 2025-08-04 06:52_

We could update the parameters on the server method to accept the parts of `WorkspaceDiagnosticParams` instead but I'm fine either way.

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:626 on 2025-08-04 10:01_

Oh, I now remember why it is necessary. The key difference is that this method doesn't await the response because we need to handle the response specially (it's long polling)

---

_@MichaReiser reviewed on 2025-08-04 10:01_

---

_Merged by @MichaReiser on 2025-08-04 10:26_

---

_Closed by @MichaReiser on 2025-08-04 10:26_

---

_Branch deleted on 2025-08-04 10:26_

---

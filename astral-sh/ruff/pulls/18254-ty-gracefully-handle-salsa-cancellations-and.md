```yaml
number: 18254
title: "[ty] Gracefully handle salsa cancellations and panics in background request handlers"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/panic-handlers-backround-threads
created_at: 2025-05-22T13:02:36Z
updated_at: 2025-05-26T12:37:51Z
url: https://github.com/astral-sh/ruff/pull/18254
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Gracefully handle salsa cancellations and panics in background request handlers

---

_Pull request opened by @MichaReiser on 2025-05-22 13:02_

## Summary

Run the request and notification handlers in a `catch_unwind` that ensures that a handler failure doesn't tear down the entire server process. ty now also responds with an appropriate error instead of just never responding to the client's request. This has the advantage that e.g. VS code stops sending new diangostic pull requests for a file on which the server crashed before.

I considered wrapping local notifications handlers in `catch_unwind` but decided against it. Local notifications are used to update the server state. An out of date server state can lead to all sort of errors because the ranges are of. That's why I think it's better to still crash the server.

## Test Plan

* Tested that ty shows an error pop up and the backtrace when a background task panics. The server keeps running
* Tested that ty shows an error error pop up and the backtrace when a local task panics. the server crashes.


---

_Label `server` added by @MichaReiser on 2025-05-22 13:02_

---

_Label `ty` added by @MichaReiser on 2025-05-22 13:02_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:79 on 2025-05-22 13:04_

I decided to remove all `Result`s from here for now because the server already uses many salsa APIs that aren't wrapped in a `salsa::Cancelled::catch`. Instead, I decided that the server request handlers catch salsa cancellations (only necessary for background tasks because:

* Only local tasks can mutate the DB
* All local tasks run on the main loop thread.


---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:111 on 2025-05-22 13:05_

The span is very useful and tracing is just way too verbose.

---

_Comment by @github-actions[bot] on 2025-05-22 13:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:171 on 2025-05-22 13:05_

I leave this for another PR, there's already enough going on in this PR

---

_Renamed from "[ty] Abort process if worker thread panics" to "[ty] Gracefully handle salsa cancellations and panics in background handlers" by @MichaReiser on 2025-05-22 13:07_

---

_@MichaReiser reviewed on 2025-05-22 13:07_

---

_Marked ready for review by @MichaReiser on 2025-05-22 13:08_

---

_Review requested from @carljm by @MichaReiser on 2025-05-22 13:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-22 13:08_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-22 13:08_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-22 13:08_

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-22 13:08_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-22 13:08_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-22 13:08_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-22 13:08_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-05-22 13:08_

---

_Comment by @github-actions[bot] on 2025-05-22 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[ty] Gracefully handle salsa cancellations and panics in background handlers" to "[ty] Gracefully handle salsa cancellations and panics in background requests" by @MichaReiser on 2025-05-23 09:39_

---

_Review comment by @dcreager on `crates/ty_server/src/server/api.rs`:16 on 2025-05-23 15:17_

nit: This looks like it should go down in the next section with the `super` imports, since it's importing from the current crate

---

_Review comment by @dcreager on `crates/ty_project/src/db.rs`:79 on 2025-05-23 15:20_

I'm not hugely familiar with this part of the code, but this rationale sounds reasonable

---

_@dcreager approved on 2025-05-23 15:20_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:184 on 2025-05-26 10:21_

nit: we could probably use `LSPResult::with_failure_code`?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:125 on 2025-05-26 10:26_

Can you make this change for `ruff_server` as well?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:234 on 2025-05-26 10:28_

I'd recommend updating the message to highlight this is related to the "panic" instead of an error. It would be useful when looking at the log messages to understand the origin of the message.

---

_@dhruvmanila approved on 2025-05-26 10:41_

Looks good!

---

_@MichaReiser reviewed on 2025-05-26 11:38_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:125 on 2025-05-26 11:38_

I plan to backport this entire stack to ruff

---

_@MichaReiser reviewed on 2025-05-26 12:05_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:184 on 2025-05-26 12:05_

Oh, I didn't know about this method. I'll leave it as is because I use the lsp_server `Error` type in https://github.com/astral-sh/ruff/pull/18273 

---

_Renamed from "[ty] Gracefully handle salsa cancellations and panics in background requests" to "[ty] Gracefully handle salsa cancellations and panics in background request handlers" by @MichaReiser on 2025-05-26 12:11_

---

_Merged by @MichaReiser on 2025-05-26 12:37_

---

_Closed by @MichaReiser on 2025-05-26 12:37_

---

_Branch deleted on 2025-05-26 12:37_

---

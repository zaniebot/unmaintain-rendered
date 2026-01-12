```yaml
number: 18627
title: Support cancellation requests
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
assignees: []
merged: true
base: main
head: lsp-refactr
created_at: 2025-06-11T13:29:43Z
updated_at: 2025-06-12T20:08:44Z
url: https://github.com/astral-sh/ruff/pull/18627
synced_at: 2026-01-12T15:56:23Z
```

# Support cancellation requests

---

_@MichaReiser_

## Summary

This PR adds support for the LSP's cancellation requests. 

This is mainly a backport of the following two ty PRs:

* https://github.com/astral-sh/ruff/pull/18273
* https://github.com/astral-sh/ruff/pull/18211

Unlike ty, this PR doesn't add support for retries. Retries aren't necessary because
Ruff doesn't have to handle Salsa cancellations


* It removes `Requester`, `Responder` and `Notifier` and unifies them under a single `Client` API. I found them more confusing than helpful, and I don't see any reason why we should restrict background tasks from sending requests.
* It removes `ClientSender`: This is mostly a fallout of the above.
* Split `IOThreads` out of `Connection`: This removes the entire complexity around weak references because scoping rules now ensure that `Connection` (and its senders) are dropped before we join the thread.
* Remove the global `MESSENGER` and instead require callers to explicitly pass a `Client`. This helped find `show_message` call sites where `MESSENGER` wasn't initialized. It also removes global state, which is always good
* Responses are sent to the main loop and not directly to the client. This is required to mark the request as complete (or omit the response if the request was cancelled), and it is only possible with a mut reference to `Session` that background tasks don't have. This approach is the same as r-a's.
* Notifications and requests are still sent directly to the client to reduce load on the main loop. We could change that if we want but I decided against it for now.

## Test Plan

Started VS code with the new ruff server. 

* Verified that diagnostics still show up. 
* Verified that no ruff process is running after closing VS code
* I added a 1s sleep to the diagnostic handler. I verified that there are cancellation requests (and the requests are cancelled)


---

_Label `server` added by @MichaReiser on 2025-06-11 13:29_

---

_Comment by @github-actions[bot] on 2025-06-11 13:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-11 13:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-06-11 13:37_

---

_Review requested from @carljm by @MichaReiser on 2025-06-11 13:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-11 13:37_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-11 13:37_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-11 13:37_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-11 13:43_

---

_Review request for @carljm removed by @carljm on 2025-06-11 18:03_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:359 on 2025-06-12 09:53_

```suggestion
                        "The Ruff language server exited with a panic. See the logs for more details.",
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/request_queue.rs`:1 on 2025-06-12 10:03_

This looks exactly the same as the one in the `ty_server`, can we re-use it?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/client.rs`:1 on 2025-06-12 10:05_

This also looks the same as the one in `ty_server` except for the `retry` method, maybe this could be re-used as well?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/main_loop.rs`:217 on 2025-06-12 10:07_

If we can't re-use the client implementation, we should probably inline the single enum variant in `Event`:
```rs
#[derive(Debug)]
pub enum Event {
    /// An incoming message from the LSP client.
    Message(lsp_server::Message),

    /// Send a response to the client
    SendResponse(lsp_server::Response),
}
```

---

_@dhruvmanila approved on 2025-06-12 10:09_

Looks good! I haven't looked very closely at the code but it all looks almost same as the one in `ty_server` so I've provided a couple of recommendations to de-duplicate if possible. This doesn't need to happen in this PR and we can also open a "help-wanted" issue for this.

---

_@MichaReiser reviewed on 2025-06-12 17:14_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/client.rs`:1 on 2025-06-12 17:14_

Yes, it's almost the same with the exception that it doesn't have a `retry` function. 

It's not a "difficult" problem but there is no good place for the files that are shared between ruff and the ty server. And `Client` depends on `Session`, which makes the reuse a bit of a challenge.

---

_Closed by @MichaReiser on 2025-06-12 18:36_

---

_Reopened by @MichaReiser on 2025-06-12 18:36_

---

_Merged by @MichaReiser on 2025-06-12 20:08_

---

_Closed by @MichaReiser on 2025-06-12 20:08_

---

_Branch deleted on 2025-06-12 20:08_

---

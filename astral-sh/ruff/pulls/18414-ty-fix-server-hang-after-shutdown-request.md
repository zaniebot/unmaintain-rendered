```yaml
number: 18414
title: "[ty] Fix server hang after shutdown request"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/server-shutdown
created_at: 2025-06-01T17:16:47Z
updated_at: 2025-06-02T06:58:08Z
url: https://github.com/astral-sh/ruff/pull/18414
synced_at: 2026-01-12T15:56:18Z
```

# [ty] Fix server hang after shutdown request

---

_@MichaReiser_

## Summary

This fixes an error where the ty server didn't exit after receiving the shutdown request. 

The root cause was the panic handler which hold on to a client sender channel, preventing the io thread from exiting (because there was still the chance that we would send a message).

I tested that the server now correctly exits with VS code. However, I never see an exit notification when closing VS code, only when restarting the server with the restart command. This is consistent with ruff.




---

_Label `server` added by @MichaReiser on 2025-06-01 17:16_

---

_Label `ty` added by @MichaReiser on 2025-06-01 17:16_

---

_Label `server` added by @MichaReiser on 2025-06-01 17:16_

---

_Label `ty` added by @MichaReiser on 2025-06-01 17:16_

---

_Comment by @github-actions[bot] on 2025-06-01 17:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-06-01 17:26_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:52 on 2025-06-01 17:26_

I ended up rewriting the shutdown handling during my investigation and I sort of like this more.

---

_Marked ready for review by @MichaReiser on 2025-06-01 17:39_

---

_Review requested from @carljm by @MichaReiser on 2025-06-01 17:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-01 17:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-01 17:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-01 17:39_

---

_Renamed from "[ty] Fix server doesn't exit after shutdown" to "[ty] Fix server hang after shutdown request" by @MichaReiser on 2025-06-01 17:43_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-01 17:56_

---

_@MichaReiser reviewed on 2025-06-01 19:02_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:148 on 2025-06-01 19:02_

This is the main fix. Values assigned to `_` are dropped immediately. That means, the panic hook was always restored immediately (and then overridden again). 

> Statements which assign an expression to an underscore causes the expression to immediately drop instead of extending the expression’s lifetime to the end of the scope. This is usually unintended, especially for types like MutexGuard, which are typically used to lock a mutex for the duration of an entire scope.

It looks like Rust 1.88 will add a lint for this [source](https://doc.rust-lang.org/beta/nightly-rustc/rustc_lint/let_underscore/static.LET_UNDERSCORE_DROP.html)

The fix here is to assign the panic hook to a variable other than `_`. The `Drop` handler then restores the original panic hook, which in turn, drops our custom panic hook handler that holds on to the client


---

_@MichaReiser reviewed on 2025-06-01 19:43_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:203 on 2025-06-01 19:43_

```suggestion
        // Use a weak reference to the client because it must be dropped when exiting or the
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:56 on 2025-06-02 02:15_

Should we avoid adding the request in the queue if the server is responding right away?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:49 on 2025-06-02 02:19_

I think we should avoid adding periods to messages which is in line with how we format other messages throughout Ruff
```suggestion
                                        message: "Shutdown already requested".to_owned(),
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:65 on 2025-06-02 02:23_

I'd consider these two messages to be mutually exclusive, so maybe something like the following (appended "exiting" to both as the server would exit in both scenario):
```suggestion
                                if self.session.is_shutdown_requested() {
                                	tracing::debug!("Received exit notification, exiting");
                                } else {
                                    tracing::warn!(
                                        "Received exit notification before shutdown request, exiting"
                                    );
                                }
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:143 on 2025-06-02 02:26_

The variable assignment seems unnecessary as the `next.map(Some)` is only for a single branch, maybe we can simplify it:

```rs
        select!(
            recv(self.connection.receiver) -> msg => Ok(msg.ok().map(Event::Message)),
            recv(self.main_loop_receiver) -> event => event.map(Some),
        )
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:144 on 2025-06-02 02:29_

Why are we ignoring the error when receiving the client messages (via `connection`) but not for the main loop receiver?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:144 on 2025-06-02 02:33_

Oh, is it to explicit that the connection got closed without a proper shutdown sequence which is how it's being handled in the main loop?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:75 on 2025-06-02 02:36_

I'm assuming we don't need to modify anything in this branch mainly because the server would stop handling any request / notification from the client when shutdown has been initiated and so the server wouldn't try to send any request to the client which means there shouldn't be any client responses to handle during the shutdown process.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:53 on 2025-06-02 02:37_

```suggestion
            tracing::debug!("Received shutdown request, waiting for shutdown notification");
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api.rs`:52 on 2025-06-02 02:37_

Thank you! I like this a lot, I was wondering myself whether the custom `Connection` struct could be removed with your recent changes around cancellation / retry.

---

_@dhruvmanila approved on 2025-06-02 02:51_

Thank you for the quick fix! The issue seems very subtle, I think we should enable the lint rule when we can at least for the server code.

I've been also wondering whether it would make sense to invest some time to having code sharing between the Ruff and ty language server where we can. It might be possible to share the the infrastructure code i.e., the ones that involves creating the event loop.

I tested this in Neovim, the server exists successfully:

```
   0.454553583s DEBUG     ty:main ty_server::server::api: Received shutdown request, waiting for shutdown notification.
   0.471809916s DEBUG     ty:main ty_server::server::main_loop: Received exit notification, exiting
   0.481823416s  INFO        main ty_server: Server shut down
```

I also looked at the running processes and it now only contains the server process from `main`.

---

_@MichaReiser reviewed on 2025-06-02 06:26_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:144 on 2025-06-02 06:26_

Yes, we ignore the connection closed errors for the IO channel because that means the server exited in a non-graceful way. 

We should never see those for the main loop because we always have at least our own sender.

---

_@MichaReiser reviewed on 2025-06-02 06:28_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:75 on 2025-06-02 06:28_

We could add the handling to notifications, because they're explicitly listed in the LSP specification. It's less clear to me if client responses are excluded too and it's quiet possible that the server might send a request from a background task when the shutdown was already initialized. 

---

_Comment by @MichaReiser on 2025-06-02 06:29_

> I've been also wondering whether it would make sense to invest some time to having code sharing between the Ruff and ty language server where we can. It might be possible to share the the infrastructure code i.e., the ones that involves creating the event loop.

It might be more work than it's worth it. The request handlers, session, etc look very different between ruff and ty

---

_@MichaReiser reviewed on 2025-06-02 06:40_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:56 on 2025-06-02 06:40_

Actually, we need to register the request when using `client.respond` because it checks if the request is registered before responding. We could use `client_sender` directly to avoid the registration but I rather use the "proven" way and it's an edge case anyway, so that performance isn't that relevant. 

---

_@MichaReiser reviewed on 2025-06-02 06:42_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:65 on 2025-06-02 06:42_

I don't think they're exlusive to each other. We always exit, but we want a warning when we do so before we receive a shutdown request. We could probably return with an `Err` in that case, similar to how we error when we didn't receive an `ExitNotification`

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:65 on 2025-06-02 06:46_

Returning with an error is more conform with the LSP specification

> A notification to ask the server to exit its process. The server should exit with success code 0 if the shutdown request has been received before; otherwise with error code 1.


---

_@MichaReiser reviewed on 2025-06-02 06:46_

---

_@MichaReiser reviewed on 2025-06-02 06:47_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:75 on 2025-06-02 06:47_

I'll leave it as is. The LSP specification only mentions:

> If a server receives requests after a shutdown request those requests should error with InvalidRequest.

Processing notifications and responses is a bit wastefull but shouldn't harm too much (they are all very short)

---

_@MichaReiser reviewed on 2025-06-02 06:48_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:144 on 2025-06-02 06:48_

I added a comment

---

_Merged by @MichaReiser on 2025-06-02 06:57_

---

_Closed by @MichaReiser on 2025-06-02 06:57_

---

_Branch deleted on 2025-06-02 06:57_

---

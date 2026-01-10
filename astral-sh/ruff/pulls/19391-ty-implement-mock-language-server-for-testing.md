```yaml
number: 19391
title: "[ty] Implement mock language server for testing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: dhruv/mock-server
created_at: 2025-07-17T04:26:42Z
updated_at: 2025-07-24T05:25:18Z
url: https://github.com/astral-sh/ruff/pull/19391
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Implement mock language server for testing

---

_Pull request opened by @dhruvmanila on 2025-07-17 04:26_

## Summary

Closes: astral-sh/ty#88

This PR implements an initial version of a mock language server that can be used to write e2e tests using the real server running in the background.

The way it works is that you'd use the `TestServerBuilder` to help construct the `TestServer` with the setup data. This could be the workspace folders, populating the file and it's content in the memory file system, setting the right client capabilities to make the server respond correctly, etc. This can be expanded as we write more test cases.

There are still a few things to follow-up on:
- ~In the `Drop` implementation, we should assert that there are no pending notification, request and responses from the server that the test code hasn't handled yet~ Implemented in [`afd1f82` (#19391)](https://github.com/astral-sh/ruff/pull/19391/commits/afd1f82bdee55ea0b5426e5ee49867b77e0c1f0a)
- Reduce the setup boilerplate in any way we can
- Improve the final assertion, currently I'm just snapshotting the final output

## Test Plan

Written a few test cases.


---

_Label `server` added by @dhruvmanila on 2025-07-17 04:26_

---

_Label `testing` added by @dhruvmanila on 2025-07-17 04:26_

---

_Label `ty` added by @dhruvmanila on 2025-07-17 04:26_

---

_Comment by @github-actions[bot] on 2025-07-17 04:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-07-17 06:09_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:42 on 2025-07-17 06:09_

I would probably remove the `Option` here. It seems to complicate things unnecessarily only so that we can fallback to `OsSystem`. Doingt hsi fallback in `Server::new` seems easy enough.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:42 on 2025-07-17 06:34_

If we remove the `Option` here, then we would need to create the `OsSystem` outside i.e., in `run_server` and when creating the `TestServer`. I don't mind that but is this what you meant?

---

_@dhruvmanila reviewed on 2025-07-17 06:34_

---

_@MichaReiser reviewed on 2025-07-17 06:55_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:42 on 2025-07-17 06:55_

Yes, that's what I meant. Creating it in `run_server` seems fine to me. It's similar to what we do in the CLI where the system is one of the first things we create. Unless we can't do that because we need to use a different current directory.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/logging.rs`:18 on 2025-07-17 07:14_

The global tracing subscriber can only be set once but `Server::new` will try to set it multiple times via `TestServer::new` when there are more than 1 test to run.

---

_@dhruvmanila reviewed on 2025-07-17 07:14_

---

_Comment by @github-actions[bot] on 2025-07-17 08:32_

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

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:326 on 2025-07-17 14:50_

Do we have any precedence on how to handle paths in tests on both Unix and Windows? I'm not exactly sure but I'm guessing that Windows failure are due to the fact that `Url::try_from_path` would fail on this path when running on Windows.

---

_@dhruvmanila reviewed on 2025-07-17 14:50_

---

_Marked ready for review by @dhruvmanila on 2025-07-17 14:50_

---

_Review requested from @carljm by @dhruvmanila on 2025-07-17 14:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-17 14:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-17 14:51_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-17 14:51_

---

_Review comment by @MichaReiser on `crates/ty_server/src/lib.rs`:49 on 2025-07-17 17:28_

Nit: I find `fallback_system` a bit misleading. I think I would call it `native_system` or `underlying_system` instead (here and everywhere else). The reason I find `fallback` confusing is because there are many operations where the LSP System doesn't customize the system at all and simply forwards to it. It's not falling back to it

---

_Review comment by @MichaReiser on `crates/ty_server/src/logging.rs`:20 on 2025-07-17 17:31_

Hmm. I'm not sure this is the right solution. I'm leaning towards passing an enum `ServerLogging::On/Off` to `Server::run` instead. This allows your testing framework to control the logging (e.g. we probably want to default to `Debug` instead of `Info`) by initializing the logging before running any tests. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:316 on 2025-07-17 17:32_

Nit: I would lean towards making `with_memory_system` optional. It seems a bit verbose and ideally wouldn't be necessary in every test. The `TestServerBuilder` can simply default to `InMemorySystem::default` unless it gets overriden.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:697 on 2025-07-18 07:08_

Nit: I would call these methods `enable_...`. It's otherwise confusing why `with_client_capabilities` replaces all capabilities but `with_did_change_watched_files` doesn't? 

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:104 on 2025-07-18 07:46_

Should we move the `drain` to after the shutdown handling because the server could still be sending requests to the client during the shutdown sequence if it wnated to.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:125 on 2025-07-18 07:50_

I think this could lead to an infinite hang if the server didn't respond to the shutdown request (because it keeps running). 

I suggest that you manually drop the `ClientConnection` before joining the server. This should force the server to exit immediately because there are no more incoming message and only then join the server. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:123 on 2025-07-18 07:51_

The message here is missleading. `join` always joins the thread but it returns an error if the server thread panicked. We should update the message to make this clear

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:155 on 2025-07-18 07:52_

Should we change the argument to take a `Vec<(WorkspaceFolder, ClientOptions)>` instead so that we don't need the assertion? The extra allocations shouldn't matter here.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:153 on 2025-07-18 07:54_

Nit: You could consider extracting a `ServerState` struct that implements `Default` that includes all fields except `server_thread`. That should also allow you to derive the `Debug` implementation.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:305 on 2025-07-18 07:59_

The only reason `send` can fail is if the server thread dropped the `ClientReceiver`. The only cases where this should happen is if the server thread panicked or shutdown. 

I suggest that we handle the error explicitly here by checking if the server thread is still running.
* if the server is still running, panic with a message saying that the server droped the client receiver.
* if the server isn't running anymore. That means the server exited for whatever reason or panicked. In this case, join the server thread. If it returns an `Err`, the server panicked -> propagate the panic. If it exited with `Ok`, panic with a message saying that the server is shutdown.


This will probably require some changes to `Drop` to skip the exit sequence and joining if the join handle is None.

But it should allow you to remove the `Result` return type.

I also suggest attributing this function with `#[track_caller]` and including the request method in the panic messages.

We should do the same for `send_notification` and `get_response` 

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:329 on 2025-07-18 08:02_

Nit: Maybe `await_response` to make it clear that this call blocks (same for notifications)

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:347 on 2025-07-18 08:03_


```suggestion
            let notification = self
                .notifications
                .iter()
                .position(|notification| N::METHOD == notification.method)
                .and_then(|index| self.notifications.remove(index));

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:386 on 2025-07-18 08:03_


```suggestion
            "Did not get a notification of type `{}` after 10 retries",
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:376 on 2025-07-18 08:04_


```suggestion
            let request = self
                .requests
                .iter()
                .position(|request| R::METHOD == request.method)
                .and_then(|index| self.requests.remove(index));
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:367 on 2025-07-18 08:06_

Do we need that many retries or are the tests flaky otherwise? Waiting for 20s seems pretty long. 

If yes, I then suggest to log an info message with `tracing` before retrying so that a dev doesn't have to wait 20s to find out which notification took more than 20s to receive.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:437 on 2025-07-18 08:08_

Same as for send. We should handle the case here where the server stopped or dropped the `Sender` end of the channel and panic in those cases. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:308 on 2025-07-18 08:12_

This is more of an extension but it would be nice if the server tracked the `pending_requests` (`FxHashMap<id, &'static str>` where the value is the method).

* `get_response` should panic if the server receives a response for a request not in `pending_requests` (that means the response was sent twice, e.g. after cancellation)
* `Drop` should probably panic if there are any pending requests? I'm not entirely sure about that as it isn't a LSP spec violation. We might at least want to warn using tracing

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:490 on 2025-07-18 08:14_

Let's log a tracing warning or even error for those cases to make it easier to track down what's happening

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:500 on 2025-07-18 08:15_

It's not quiet clear to me how this should be used. Ideally, a test is able to customize any of those (and they are by calling `get_request`). 

An alternative way of implementing this is to simply have the test call `get_request` and then call `handle_workspace_configuration_request(request)` itself. Should we remove this for now?

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:245 on 2025-07-18 08:20_

Aren't document versions per document (instead of global?)

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:323 on 2025-07-18 08:21_

The snapshot captures more than just the capabilities

```suggestion
        insta::assert_json_snapshot!("initialization", initialization_result);
```

---

_@MichaReiser approved on 2025-07-18 08:23_

Nice, this is awesome! 

I've a few suggestions on how the error handling and asserting some basic constraints of the LSP protocol can be improved. But I don't mind if you decide to defer those to a follow up PR. 

---

_Review comment by @dhruvmanila on `crates/ty_server/src/logging.rs`:20 on 2025-07-18 11:11_

The issue here is that the global tracing subscriber can only be set once and each test will try to set it, so we need to make sure that the logging is initialized only once. Using your suggested approach (`ServerLogging`), I guess we would need to make it `Off` by default and the developer can turn it `On` when necessary e.g., to debug a test.

---

_@dhruvmanila reviewed on 2025-07-18 11:11_

---

_@dhruvmanila reviewed on 2025-07-18 11:12_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:316 on 2025-07-18 11:12_

It is optional, I'll remove this call from this test case.

---

_@dhruvmanila reviewed on 2025-07-18 11:14_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:697 on 2025-07-18 11:14_

I'm not sure what's confusing here, the `with_client_capabilities` should be replacing the entire capability set while `with_` prefixed specific capability should only be replacing that specific one. I'm fine with renaming it to `enable_` regardless.

---

_@MichaReiser reviewed on 2025-07-18 11:25_

---

_Review comment by @MichaReiser on `crates/ty_server/src/logging.rs`:20 on 2025-07-18 11:25_

I don't mind having a `OnceLock` inside the `TestServer`. I just don't think it's the right approach in `Server::run`

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:104 on 2025-07-18 11:33_

I think we should do this first because if we reached here then the test case didn't handle some messages.

---

_@dhruvmanila reviewed on 2025-07-18 11:33_

---

_@dhruvmanila reviewed on 2025-07-18 11:34_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:125 on 2025-07-18 11:34_

Ah, good point. I think I faced this when the server didn't respond to the shutdown request because the workspaces initialization was yet to be completed. I didn't realize it was due to this.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:104 on 2025-07-18 11:45_

We might then need to do it twice?

---

_@MichaReiser reviewed on 2025-07-18 11:45_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:104 on 2025-07-18 11:47_

Oh, I think I might have misunderstood what it does. It simply drops all messages. Shouldn't it be an error if a test doesn't handle all messages? We could add a method that drops all messages but I would sort of expect that all messages are handled and any unhandled message is an error (because it didn't fully test the flow?)

---

_@MichaReiser reviewed on 2025-07-18 11:47_

---

_@dhruvmanila reviewed on 2025-07-18 11:51_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:104 on 2025-07-18 11:51_

It doesn't drop all the message, it basically keeps asking the server for the messages via the `self.receive` call until we stop receiving any messages from the server which records them in `self.requests`, `self.notifications` and `self.responses` and at the end there's a `self.assert_no_pending_messages` call that asserts that these data structures are empty.

---

_@dhruvmanila reviewed on 2025-07-18 11:54_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:153 on 2025-07-18 11:54_

I like the idea but it just adds a layer of indirection which doesn't seem that useful. As such, we won't be able to implement the `Default` because `workspace_configurations` needs to be provided.

---

_@dhruvmanila reviewed on 2025-07-18 11:58_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:367 on 2025-07-18 11:58_

I don't think it's required, it's just an arbitrary number. I'll reduce it to 5 and add the tracing messages.

---

_@dhruvmanila reviewed on 2025-07-18 12:02_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:104 on 2025-07-18 12:02_

This split is necessary because we should only panic after the server thread is joined to avoid unnecessary panic messages.

---

_@dhruvmanila reviewed on 2025-07-18 12:03_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:123 on 2025-07-18 12:03_

Updated the message to:

```rs
panic!("Panic in the server thread: {err:?}");
```

---

_@dhruvmanila reviewed on 2025-07-18 12:10_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:490 on 2025-07-18 12:10_

Returning an `Err` in this case.

---

_@dhruvmanila reviewed on 2025-07-18 12:11_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:245 on 2025-07-18 12:11_

They are, I was just a bit lazy to implement per document tracking. I think as long as the document version change and are incremental, it should be fine?

---

_@MichaReiser reviewed on 2025-07-18 12:13_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:245 on 2025-07-18 12:13_

We could also just ask the test to provide the versions instead so that they can emulate exactly the behavior they want

---

_@dhruvmanila reviewed on 2025-07-18 12:14_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:500 on 2025-07-18 12:14_

Yeah, this was present before the `wait_until_workspaces_are_initialized` was implemented. The idea was that `self.receive` would handle certain requests made by the server but it would probably be confusing if the test wants to handle them on it's own. I'll remove this.

---

_@dhruvmanila reviewed on 2025-07-18 12:19_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/logging.rs`:20 on 2025-07-18 12:19_

I'm not sure how would that work. The tracing is initialized from `Server::new` (and not `Server::run`) and the `TestServer` calls the `Server::new` method everytime the test server is created.

---

_@dhruvmanila reviewed on 2025-07-18 13:27_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:490 on 2025-07-18 13:27_

Actually, I'm thinking of making it a warning instead because otherwise the server would use the current working directory for which it will send the `workspace/configuration` request to the client and the test will fail because the user didn't provide it.

---

_@dhruvmanila reviewed on 2025-07-21 09:20_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:305 on 2025-07-21 09:20_

I've made this change, I'd find it useful if you could take a quick look at it.

---

_Review comment by @MichaReiser on `crates/ty_server/src/logging.rs`:20 on 2025-07-21 11:08_

Something along these lines:

```
Subject: [PATCH] [ty] Use `ThinVec` in various places
---
Index: crates/ty_server/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/lib.rs b/crates/ty_server/src/lib.rs
--- a/crates/ty_server/src/lib.rs	(revision a7aada5ee7426039e114f49265e4d4c25f8239c1)
+++ b/crates/ty_server/src/lib.rs	(date 1753095907066)
@@ -48,9 +48,14 @@
     // This is to complement the `LSPSystem` if the document is not available in the index.
     let fallback_system = Arc::new(OsSystem::new(cwd));
 
-    let server_result = Server::new(worker_threads, connection, fallback_system)
-        .context("Failed to start server")?
-        .run();
+    let server_result = Server::new(
+        worker_threads,
+        connection,
+        fallback_system,
+        server::Logging::Initialize,
+    )
+    .context("Failed to start server")?
+    .run();
 
     let io_result = io_threads.join();
 
Index: crates/ty_server/src/test.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/test.rs b/crates/ty_server/src/test.rs
--- a/crates/ty_server/src/test.rs	(revision a7aada5ee7426039e114f49265e4d4c25f8239c1)
+++ b/crates/ty_server/src/test.rs	(date 1753096005804)
@@ -118,7 +118,12 @@
             let worker_threads = NonZeroUsize::new(1).unwrap();
             let test_system = Arc::new(TestSystem::new(memory_system));
 
-            match Server::new(worker_threads, server_connection, test_system) {
+            match Server::new(
+                worker_threads,
+                server_connection,
+                test_system,
+                crate::server::Logging::Inherit,
+            ) {
                 Ok(server) => {
                     if let Err(err) = server.run() {
                         panic!("Server stopped with error: {err:?}");
Index: crates/ty_server/src/server.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/server.rs b/crates/ty_server/src/server.rs
--- a/crates/ty_server/src/server.rs	(revision a7aada5ee7426039e114f49265e4d4c25f8239c1)
+++ b/crates/ty_server/src/server.rs	(date 1753095974552)
@@ -26,6 +26,12 @@
 pub(crate) use main_loop::{Action, ConnectionSender, Event, MainLoopReceiver, MainLoopSender};
 pub(crate) type Result<T> = std::result::Result<T, api::Error>;
 
+#[derive(Copy, Clone, Debug, Eq, PartialEq)]
+pub(crate) enum Logging {
+    Initialize,
+    Inherit,
+}
+
 pub(crate) struct Server {
     connection: Connection,
     client_capabilities: ClientCapabilities,
@@ -40,6 +46,7 @@
         worker_threads: NonZeroUsize,
         connection: Connection,
         native_system: Arc<dyn System + 'static + Send + Sync + RefUnwindSafe>,
+        logging: Logging,
     ) -> crate::Result<Self> {
         let (id, init_value) = connection.initialize_start()?;
         let init_params: InitializeParams = serde_json::from_value(init_value)?;
@@ -76,10 +83,15 @@
         let (main_loop_sender, main_loop_receiver) = crossbeam::channel::bounded(32);
         let client = Client::new(main_loop_sender.clone(), connection.sender.clone());
 
-        crate::logging::init_logging(
-            global_options.tracing.log_level.unwrap_or_default(),
-            global_options.tracing.log_file.as_deref(),
-        );
+        match logging {
+            Logging::Initialize => {
+                crate::logging::init_logging(
+                    global_options.tracing.log_level.unwrap_or_default(),
+                    global_options.tracing.log_file.as_deref(),
+                );
+            }
+            Logging::Inherit => {}
+        }
 
         tracing::debug!("Version: {version}");
 

```

Now, for tests. Setup the logging automatically in `TestServerBuilder` or create a `setup_logging` helper function that sets up logging for a test (this is what we use in `ruff_db`). 

The `setup_logging` function would use a `OnceLock` internally to ensure we only setup the test logging once.

---

_@MichaReiser reviewed on 2025-07-21 11:08_

---

_@MichaReiser reviewed on 2025-07-21 11:09_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:326 on 2025-07-21 11:09_

As discussed on Discord. I suggest to either use a temp directory or extend the `InMemorySystem` to support non-unix paths.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:193 on 2025-07-21 11:12_

Nit: The `initialize` function can compute `workspace_folders` from iterating over `workspace_configurations` (move line 133 into `initialize)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-21 13:42_

---

_@dhruvmanila reviewed on 2025-07-22 08:58_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/logging.rs`:20 on 2025-07-22 08:58_

Thanks, I've added this.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:679 on 2025-07-22 09:07_

We should probably skip joining the server thread when the client thread is already panicking. Triggering the panic on line 659 will only lead to a double panic which are very annoying to debug in Rust (Rust doesn't like that at all). That's why I think it's best to move the `std::thread::panicking` up before taking the server thread.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:55 on 2025-07-22 09:08_

Should we default to `Debug`?  I don't find `Info` sufficient when working on the LSP

---

_Comment by @MichaReiser on 2025-07-22 14:18_

I'm taking a look at the failing windows test

---

_Comment by @MichaReiser on 2025-07-22 14:27_

This works for me:

```diff
Subject: [PATCH] Add Micro Benchmark

This PR adds a microbenchmark for the Ruff linter. Adding microbenchmark for other tools
should be straightforward.
---
Index: crates/ty_server/src/test.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/test.rs b/crates/ty_server/src/test.rs
--- a/crates/ty_server/src/test.rs	(revision 07e2747a21e17da81ff3c9433e27721bb8a9d61e)
+++ b/crates/ty_server/src/test.rs	(date 1753194432043)
@@ -13,7 +13,7 @@
 use std::time::Duration;
 use std::{fmt, fs};
 
-use anyhow::{Context, Result};
+use anyhow::{Context, Result, anyhow};
 use crossbeam::channel::RecvTimeoutError;
 use insta::internals::SettingsBindDropGuard;
 use lsp_server::{Connection, Message, RequestId, Response, ResponseError};
@@ -836,15 +836,16 @@
         })?;
 
         let mut settings = insta::Settings::clone_current();
-        settings.add_filter(&tempdir_filter(&project_dir), "<temp_dir>/");
+        settings.add_filter(&tempdir_filter(project_dir.as_str()), "<temp_dir>/");
         // TODO: Is this required?
-        // settings.add_filter(
-        //     &tempdir_filter(
-        //         Url::from_file_path(project_dir.as_std_path())
-        //             .context("Failed to convert project path to URL")?,
-        //     ),
-        //     "file://<temp_dir>/",
-        // );
+        settings.add_filter(
+            &tempdir_filter(
+                Url::from_file_path(project_dir.as_std_path())
+                    .map_err(|()| anyhow!("Failed to convert root directory to url"))?
+                    .path(),
+            ),
+            "/<temp_dir>/",
+        );
         settings.add_filter(r#"\\(\w\w|\s|\.|")"#, "/$1");
         settings.add_filter(
             r#"The system cannot find the file specified."#,
@@ -865,6 +866,6 @@
     }
 }
 
-fn tempdir_filter(path: &SystemPath) -> String {
-    format!(r"{}\\?/?", regex::escape(path.as_str()))
+fn tempdir_filter(path: impl AsRef<str>) -> String {
+    format!(r"{}\\?/?", regex::escape(path.as_ref()))
 }

```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:317 on 2025-07-22 14:43_

I think I'd prefer if the tests were written as integration tests in the `tests` folder. See the `ty/cli/main.rs` for how to share code between integration tests. Unless this forces us to make many more types public that otherwise wouldn't have to be.

My main thinking here is that these are integration tests and the `snapshots` folder is a bit distracting in the middle of rust modules (this could also be solved by specifying another path in the insta settings)

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:809 on 2025-07-22 14:47_

Nit: I was surprised that the insta scope is stored on `TestDir`. That's not where I would have looked for it. 
Maybe rename it to `TestScope` or `TestContext` to make it clear that it isn't the `TempDir` type (or a thin wrapper around it)

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:229 on 2025-07-22 14:50_

I see that `receive` panics if the server is disconnected. I think this is a problem, considering that `drain_messages` is called in `Drop`. 

I think we should propagate the `Disconnect` error from `receive` and explicitly handle it here (immediately exit the loop).

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:229 on 2025-07-22 14:51_

Should we log a warning or debug message for any of those unhandled errors?

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:333 on 2025-07-22 15:01_

Nit: Should we flip the order here: First try to get a response from `self.responses` and only call `self::Receive` if we haven't received such a response yet? That should help speed up tests because it avoids running into the `Timeout`

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:372 on 2025-07-22 15:10_

Same as for response. Should we flip the order here to only call `receive` if ty didn't find any notification matching the requested method.

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:432 on 2025-07-22 15:16_

Nit: We could try to poll more messages in the `Ok` case to ensure the channel is empty before returning. It could otherwise mean that `response` fails if there are 6 incoming messages and the expected response is the last one.


```suggestion
        Ok(message) => {
            self.handle_message()?;

            for message in self.client_connection.as_ref().unwrap.receiver.try_iter() {
                ...
            }
        }
```

I see that this might be somewhat tricky due to borrowing. You might need to explicitly pass `requests`, `responses`, ... to `handle_message` to avoid the mutual borrow...

Or, since this isn't performance sensitive. Collect all messages into a `Vec`, then iterate over the `Vec` and call `handle_message` for each (the only thing we need to be careful about with this approach is to not drop any messages)

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:559 on 2025-07-22 15:20_

I'm still leaning towards making `version` an argument here so that the test is in full control (and other requests that require a version) but happy to defer to you

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:645 on 2025-07-22 16:18_


```suggestion
        let shutdown_error = self
            .server_thread
            .and_then(|_| {
                
            });
```

Or use a normal `if ... else`

```rust
let mut shutdown_error = None;

if self.server_thread.is_some() {
    let shutdown_id = self.send_request::<Shutdown>(());
    match self.await_response::<()>(shutdown_id) {
        Ok(()) => {
            self.send_notification::<Exit>(());
        }
        Err(err) => {
            shutdown_error = Some(format!("Failed to get shutdown response: {err:?}"));
        }
    }
}
```

---

_@MichaReiser approved on 2025-07-22 16:19_

This is great. I've a few more inline suggestions

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:55 on 2025-07-23 04:50_

Sounds reasonable, it's just that it will take up line spaces in CI because every test will add messages.

---

_@dhruvmanila reviewed on 2025-07-23 04:50_

---

_@dhruvmanila reviewed on 2025-07-23 04:52_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:317 on 2025-07-23 04:52_

Yeah, I wanted this to be integration tests but it will require making `ClientOptions` and everything inside it as publicly available so that it can be used to specify workspace options in the test server.

---

_@dhruvmanila reviewed on 2025-07-23 04:53_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:809 on 2025-07-23 04:53_

Renamed it to `TestContext`.

---

_@dhruvmanila reviewed on 2025-07-23 05:08_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:229 on 2025-07-23 05:08_

Made this change.

---

_@dhruvmanila reviewed on 2025-07-23 05:10_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:559 on 2025-07-23 05:10_

Yeah, I think that would be better, I've added the `version` parameter

---

_@dhruvmanila reviewed on 2025-07-23 05:18_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:432 on 2025-07-23 05:18_

I think this is mostly achieved via `loop { ... }` in `await_response` and the retry logic in `await_notification` and `await_request`.

---

_@MichaReiser reviewed on 2025-07-23 05:47_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:432 on 2025-07-23 05:47_

It is in `await_response` because it loops forever (is this intentional?). It isn't fully for `await_notification` and `await_request` where more than 5 incoming messages (e.g. 5 incoming responses) would result in `receive_request` returning an `Err` even if it would only need to pull one more time.


---

_@MichaReiser reviewed on 2025-07-23 05:48_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:427 on 2025-07-23 05:48_

You may want to `Box` `TestServerError`. 

---

_@MichaReiser reviewed on 2025-07-23 05:50_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:55 on 2025-07-23 05:50_

Doesn't `cargo nextest` and `cargo test` capture the output of successful tests by default and only print the output of failing tests?

If this is indeed a problem, then I suggest that test authors have to call `setup_tracing` if they want to see logs in their test. Because using `INFO` doesn't fully solve the CI problem but it's also not detailed enough to be very useful

---

_@dhruvmanila reviewed on 2025-07-23 06:16_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:55 on 2025-07-23 06:16_

Oh, you're right. The way I run test (via rust-analyzer) it passes the `--show-output` flag which is why I've been seeing those trace logs for every tests. I'll change the log level to debug.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:432 on 2025-07-23 06:21_

This logic is directly borrowed from Starlark LSP test server. I'm not sure what's the best way to keep polling for more messages would be that's generic over requests, notifications and responses and also avoid waiting too long. I'd prefer we improve this logic iteratively as we add more tests. So, I'd leave this as is for now.

---

_@dhruvmanila reviewed on 2025-07-23 06:21_

---

_@dhruvmanila reviewed on 2025-07-23 06:22_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/test.rs`:333 on 2025-07-23 06:22_

This sounds reasonable, I've made this change.

---

_@MichaReiser reviewed on 2025-07-23 06:32_

---

_Review comment by @MichaReiser on `crates/ty_server/src/test.rs`:432 on 2025-07-23 06:32_

Fair enough. My suggestion above should give a pretty good experience as it pulls all available messages. The retry logic is then only necessary in cases where we run into a timeout:


```patch
Index: crates/ty_server/src/test.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/test.rs b/crates/ty_server/src/test.rs
--- a/crates/ty_server/src/test.rs	(revision 07e2747a21e17da81ff3c9433e27721bb8a9d61e)
+++ b/crates/ty_server/src/test.rs	(date 1753252303307)
@@ -35,6 +35,7 @@
     WorkspaceClientCapabilities, WorkspaceFolder,
 };
 use ruff_db::system::{OsSystem, SystemPath, SystemPathBuf, TestSystem};
+use rustc_hash::FxHashMap;
 use serde::de::DeserializeOwned;
 use tempfile::TempDir;
 
@@ -105,7 +106,7 @@
     version_counter: i32,
 
     /// A mapping of request IDs to responses received from the server
-    responses: HashMap<RequestId, Response>,
+    responses: FxHashMap<RequestId, Response>,
 
     /// An ordered queue of all the notifications received from the server
     notifications: VecDeque<lsp_server::Notification>,
@@ -422,14 +423,18 @@
     fn receive(&mut self, timeout: Option<Duration>) -> Result<(), TestServerError> {
         static DEFAULT_TIMEOUT: Duration = Duration::from_secs(1);
 
-        match self
-            .client_connection
-            .as_ref()
-            .unwrap()
-            .receiver
-            .recv_timeout(timeout.unwrap_or(DEFAULT_TIMEOUT))
-        {
-            Ok(message) => self.handle_message(message),
+        let receiver = self.client_connection.as_ref().unwrap().receiver.clone();
+
+        match receiver.recv_timeout(timeout.unwrap_or(DEFAULT_TIMEOUT)) {
+            Ok(message) => {
+                self.handle_message(message)?;
+
+				 // drain all other available messages without waiting
+                for message in receiver.try_iter() {
+                    self.handle_message(message)?;
+                }
+
+                Ok(())
+            }
             Err(RecvTimeoutError::Timeout) => Err(TestServerError::RecvTimeoutError),
             Err(RecvTimeoutError::Disconnected) => {
                 self.panic_on_server_disconnect();
```



---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:317 on 2025-07-23 06:34_

I see. I don't think I would mind this overly much but maybe worth a separate PR so that it's clear what we need to make public

---

_@MichaReiser reviewed on 2025-07-23 06:34_

---

_Merged by @dhruvmanila on 2025-07-23 06:56_

---

_Closed by @dhruvmanila on 2025-07-23 06:56_

---

_Branch deleted on 2025-07-23 06:56_

---

_@dhruvmanila reviewed on 2025-07-24 05:24_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:326 on 2025-07-24 05:24_

I settled on using a temp directory instead.

---

_@dhruvmanila reviewed on 2025-07-24 05:25_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/logging.rs`:18 on 2025-07-24 05:25_

This was reverted and the tracing subscriber is registered only once during tests via the `setup_tracing` in `test.rs`.

---

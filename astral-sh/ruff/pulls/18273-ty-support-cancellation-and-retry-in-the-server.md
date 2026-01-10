```yaml
number: 18273
title: "[ty] Support cancellation and retry in the server"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/server-cancellation-handling
created_at: 2025-05-23T09:15:52Z
updated_at: 2025-05-28T08:59:30Z
url: https://github.com/astral-sh/ruff/pull/18273
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Support cancellation and retry in the server

---

_Pull request opened by @MichaReiser on 2025-05-23 09:15_

## Summary

This PR implements the following releated features:

* Support re-trying requests after they failed due to a salsa Cancellation (some content in the db was modified)
* Support for the LSP cancel request: Don't retry requests that were cancelled. Don't run background requests that were cancelled before they run on the background thread

This required significant refactoring of the server's request/notification/respond model because tracking the pending requests requires access to `Session`, which we don't have in background tasks. 

This PR:

* It removes `Requester`, `Responder` and `Notifier` and unifies them under a single `Client` API. I found them more confusing than helpful, and I don't see any reason why we should restrict background tasks from sending requests.
* It removes `ClientSender`: This is mostly a fallout of the above.
* Split `IOThreads` out of `Connection`: This removes the entire complexity around weak references because scoping rules now ensure that `Connection` (and its senders) are dropped before we join the thread.
* Remove the global `MESSENGER` and instead require callers to explicitly pass a `Client`. This helped find `show_message` call sites where `MESSENGER` wasn't initialized. It also removes global state, which is always good
* Responses are sent to the main loop and not directly to the client. This is required to mark the request as complete (or omit the response if the request was cancelled), and it is only possible with a mut reference to `Session` that background tasks don't have. This approach is the same as r-a's.
* Notifications and requests are still sent directly to the client to reduce load on the main loop. We could change that if we want but I decided against it for now.

Closes https://github.com/astral-sh/ty/issues/75

## Test Plan

* I added a timeout in the background task scheduling and verified that requests weren't processed that were cancelled by the client
* I added a timeout to completions (that now has retry enabled) and verified that the request is re-executed after a salsa cancellation
* I tested that a panic on the main loop still shows a message box
* I tested that a panic in a background task shows a message box
* I tested that the server correctly restarts when issuing the restart server command


---

_Label `server` added by @MichaReiser on 2025-05-23 09:15_

---

_Label `ty` added by @MichaReiser on 2025-05-23 09:15_

---

_Label `server` added by @MichaReiser on 2025-05-23 09:15_

---

_Label `ty` added by @MichaReiser on 2025-05-23 09:15_

---

_Comment by @github-actions[bot] on 2025-05-23 09:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-05-23 09:35_

---

_Review requested from @carljm by @MichaReiser on 2025-05-23 09:35_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-23 09:35_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-23 09:35_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-23 09:35_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-26 17:52_

---

_@mscheifer reviewed on 2025-05-27 05:55_

---

_Review comment by @mscheifer on `crates/ty_server/src/lib.rs`:30 on 2025-05-27 05:55_

This should stay as `min` I think. See this PR: https://github.com/astral-sh/ruff/pull/17421

---

_Review comment by @sharkdp on `crates/ty_server/src/server/api.rs`:216 on 2025-05-27 07:18_

When reading this file top-to-bottom, this comment confused me for a bit, because of where it is located. Should this be moved one line down (into the branch), or changed to something like the following?
```suggestion
            // Check if request was canceled due to some modifications to the salsa database:
```

---

_@sharkdp approved on 2025-05-27 07:46_

Thank you very much. This all looks very reasonable, but I can not review this as deeply as it might be necessary. I'm not (yet) familiar with the LSP server code.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:97 on 2025-05-28 03:20_

Just want to understand whether the capacity of this channel is based on an analysis / experimentation or is it mainly that this should be good enough for now. I'm assuming it's the latter as it's mainly used for responses. Regardless, maybe we could extract it out as a constant (https://docs.astral.sh/ruff/rules/magic-value-comparison/ :))

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:242 on 2025-05-28 03:52_

```suggestion
/// Sends a warning to the client with a formatted message. The message is sent in a
/// `window/showMessage` notification.
#[macro_export]
macro_rules! show_warn_msg {
    ($client:expr, $msg:expr$(, $($arg:tt)*)?) => {{
        let result = $client.show_message(::core::format_args!($msg$(, $($arg)*)?), lsp_types::MessageType::WARNING);

        if let Err(err) = result {
            tracing::error!("Failed to send warning message to client: {err}");
        }
    }};
}
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:223 on 2025-05-28 03:53_

nit: I'm inclined to remove these macros and instead just make it a method on the `Client` itself like the special case of the existing `show_message` (`show_error_message`, `show_warning_message`)

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:60 on 2025-05-28 04:05_

Should we add any info for which request this error response is for? Like the request method? Or, is it that the trace logs will allow us to link the request for this response?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:79 on 2025-05-28 04:06_

I think this should be the "Client response was invalid" ?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:22 on 2025-05-28 04:30_

Curious to know how should we decide which requests are useful to retry on cancellation. Like, I think for completion it's mainly because each keystroke would change the file content which means the old completion items might be not relevant so it would be useful to re-compute the completion items.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/connection.rs`:16 on 2025-05-28 04:35_

Now that the `threads` field is removed, I wonder if it's possible to use the `lsp::Connection` instead of the custom `Connection` struct. I'm guessing probably not because of the custom `handle_shutdown` method.

---

_@dhruvmanila approved on 2025-05-28 04:40_

This is great! It simplifies a bunch of stuff.

A few questions / notes going through the diff:

* The spec mentions that cancellation would make it possible to return partial results. I don't think that's supported right now and I don't even think it's required or necessary now or in the near future until we've completed most of the capabilities. I don't know if partial results is going to be useful and if so how much.
* Does the current implementation only "records" that this request has been cancelled and not actually cancel the running thread?

---

_Comment by @MichaReiser on 2025-05-28 06:51_

> The spec mentions that cancellation would make it possible to return partial results. I don't think that's supported right now and I don't even think it's required or necessary now or in the near future until we've completed most of the capabilities. I don't know if partial results is going to be useful and if so how much.

That's correct. The current implementation always omits the entire result. Partial results would complicate things a bit because

The server can't send the cancelled response immediately if the request is already in progress. We also need to pass the `CancellationToken` to the request handler so that it can decide itself what it wants to do if it gets cancelled. I'm not sure how well that would work overall because cancellations are mainly issued after changes and changes result in a write to the db -> any db access unwinds with a `Salsa::Cancelled` payload. But definetely something worth exploring in the future

> Does the current implementation only "records" that this request has been cancelled and not actually cancel the running thread?

This is correct. However, I don't think it should matter in most cases because the main reason for a client to send a cancellation is when some content changed, which results in mutating the database. Salsa implements mutation by unwinding all other db clones when they are accessed the next time. What this means in practice is that salsa cancels in-flight requests. However, we could pass the `CancellationToken` to the request handler if we have some computationally intensive logic that doesn't perform any database access.




---

_@MichaReiser reviewed on 2025-05-28 06:52_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:97 on 2025-05-28 06:52_

Yeah, it's a made-up number. I can add a comment. I'm not a fan of the magic-value-comparison rule myself because it only adds indirection for a value that's used exactly once. 

---

_@MichaReiser reviewed on 2025-05-28 06:54_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/client.rs`:60 on 2025-05-28 06:54_

I've to study this code. It's mainly copied over. But I think it could be nice to also create a tracing span

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:22 on 2025-05-28 06:57_

I don't have a good heuristic for it yet. We don't need to retry diagnostic requests because the LSP has special support for retrying them. 

go-to-type definition are probably no longer relevant and the user can easily re-trigger them by moving the mouse a little. I think we may want retry for inlay hints (but some servers also support retrying?). I expect that it takes us a few iterations to figure out where and when we should apply retrying.

---

_@MichaReiser reviewed on 2025-05-28 06:57_

---

_@dhruvmanila reviewed on 2025-05-28 07:46_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:97 on 2025-05-28 07:46_

Adding a comment sounds reasonable as well.

---

_@dhruvmanila reviewed on 2025-05-28 07:49_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:22 on 2025-05-28 07:49_

That sounds reasonable 👍 

---

_@MichaReiser reviewed on 2025-05-28 08:23_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/connection.rs`:16 on 2025-05-28 08:23_

Possibly. I'd like to keep it as is for now. I like that `handle_shutdown` is on our `Connection` type because it makes it clear that other main loop messages won't be processed anymore

---

_@MichaReiser reviewed on 2025-05-28 08:33_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/client.rs`:60 on 2025-05-28 08:33_

I added a debug span and included the method in more messages

---

_Merged by @MichaReiser on 2025-05-28 08:59_

---

_Closed by @MichaReiser on 2025-05-28 08:59_

---

_Branch deleted on 2025-05-28 08:59_

---

```yaml
number: 11222
title: "`ruff server` no longer hangs after shutdown"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/shutdown
created_at: 2024-04-30T23:07:26Z
updated_at: 2024-05-03T01:14:34Z
url: https://github.com/astral-sh/ruff/pull/11222
synced_at: 2026-01-10T22:37:02Z
```

# `ruff server` no longer hangs after shutdown

---

_Pull request opened by @snowsignal on 2024-04-30 23:07_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11207.

The server would hang after handling a shutdown request on `IoThreads::join()` because a global sender (`MESSENGER`, used to send `window/showMessage` notifications) would remain allocated even after the event loop finished, which kept the writer I/O thread channel open.

To fix this, I've made a few structural changes to `ruff server`. I've wrapped the send/receive channels and thread join handle behind a new struct, `Connection`, which facilitates message sending and receiving, and also runs `IoThreads::join()` after the event loop finishes. To control the number of sender channels, the `Connection` wraps the sender channel in an `Arc` and only allows the creation of a wrapper type, `ClientSender`, which hold a weak reference to this `Arc` instead of direct channel access. The wrapper type implements the channel methods directly to prevent access to the inner channel (which would allow the channel to be cloned). ClientSender's function is analogous to [`WeakSender` in `tokio`](https://docs.rs/tokio/latest/tokio/sync/mpsc/struct.WeakSender.html). Additionally, the receiver channel cannot be accessed directly - the `Connection` only exposes an iterator over it.

These changes will guarantee that all channels are closed before the I/O threads are joined.

## Test Plan

Repeatedly open and close an editor utilizing `ruff server` while observing the task monitor. The net total amount of open `ruff` instances should be zero once all editor windows have closed.

The following logs should also appear after the server is shut down:

<img width="835" alt="Screenshot 2024-04-30 at 3 56 22 PM" src="https://github.com/astral-sh/ruff/assets/19577865/404b74f5-ef08-4bb4-9fa2-72e72b946695">

This can be tested on VS Code by changing the settings and then checking `Output`.

---

_Label `server` added by @snowsignal on 2024-04-30 23:07_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-30 23:07_

---

_Comment by @github-actions[bot] on 2024-04-30 23:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:116 on 2024-05-01 06:54_

Nit: Maybe rename to `close` and document that it will wait for all pending IO to complete. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:133 on 2024-05-01 06:56_

Is it necessary to use a `WeakRef` here? I would expect that we can use a strong reference here because 
a) `connection` always outlives this function call`
b) `scheduler` gets dropped at the end of the function call (which exits when it is a shutdown message)

---

_@MichaReiser approved on 2024-05-01 06:59_

---

_@snowsignal reviewed on 2024-05-02 02:25_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:116 on 2024-05-02 02:25_

That's funny because I originally called this method `close`. I can go back to that 😄 

---

_@snowsignal reviewed on 2024-05-02 02:28_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:133 on 2024-05-02 02:28_

We technically could use a strong reference but I'd rather enforce a weak-reference-only policy to guarantee that we don't have other strong references on `Connection::close`.

---

_@snowsignal reviewed on 2024-05-02 02:31_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:116 on 2024-05-02 02:31_

Just FYI I don't think this would wait on active I/O, since we disconnect the message channels before joining the thread(s).

---

_Merged by @snowsignal on 2024-05-03 01:09_

---

_Closed by @snowsignal on 2024-05-03 01:09_

---

_Branch deleted on 2024-05-03 01:09_

---

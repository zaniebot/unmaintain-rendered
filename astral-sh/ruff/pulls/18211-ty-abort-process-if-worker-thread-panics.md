```yaml
number: 18211
title: "[ty] Abort process if worker thread panics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/server-panic
created_at: 2025-05-20T06:17:00Z
updated_at: 2025-05-26T12:09:08Z
url: https://github.com/astral-sh/ruff/pull/18211
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Abort process if worker thread panics

---

_Pull request opened by @MichaReiser on 2025-05-20 06:17_

## Summary

This PR changes how ty's LSP handles panics in background worker threads.

Today, a panic in the worker thread pool gets logged (with tracing) but it tears down the worker thread on which the background task ran. 
The panic will only get surfaced once the thread pool shuts down (when `JoinHandle::join` is called). Eventually, `job_sender.send` panics
because the thread pool ran out of worker threads and the job queue overflows.

This PR aligns the behavior with rayon by aborting the entire process when any background task unexpectedly panics. 
My reasoning is that *containing* errors shouldn't be the responsibility of the thread pool. Instead, the 
request dispatching should be wrapped in a `catch_unwind` and handle any potential recovery there. This also
reveals that we don't have the same recovery for tasks running locally (on the main thread). 

I plan on adding such recovery in the server dispatch logic as a follow up (which also adds retry logic).


## Test Plan

I added a `panic` to the hover request handler and it aborted the process (which VS code then restarts up to 5 times).


---

_Review requested from @carljm by @MichaReiser on 2025-05-20 06:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-20 06:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-20 06:17_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-20 06:17_

---

_Label `server` added by @MichaReiser on 2025-05-20 06:17_

---

_Label `ty` added by @MichaReiser on 2025-05-20 06:17_

---

_@MichaReiser reviewed on 2025-05-20 06:19_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/schedule/thread/pool.rs`:54 on 2025-05-20 06:19_

The main motivation of the limit is to apply some form of back pressure. However, limiting the queue to 4 on e.g. a 12 core system feels overly strict because it means we'll drop messages as soon as 4 out of 12 threads have one message queued. We should at least allow a backlog of 2 tasks per thread.

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-20 06:19_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-20 06:19_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-20 06:19_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-20 06:19_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-05-20 06:19_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-05-20 06:19_

---

_Comment by @github-actions[bot] on 2025-05-20 06:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-20 06:27_

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

_Review comment by @BurntSushi on `crates/ty_server/src/server/schedule/thread/pool.rs`:54 on 2025-05-20 12:13_

For my own edification, can you say more about the relationship between the channel buffer size and dropping messages? Does that mean that if a channel send _would_ block (i.e., there's no receiving ready and waiting to synchronize) then that message is ~~blocked~~ dropped?

---

_@BurntSushi approved on 2025-05-20 12:17_

This makes sense to me!

---

_@MichaReiser reviewed on 2025-05-20 13:58_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/schedule/thread/pool.rs`:54 on 2025-05-20 13:58_

You're right. I was wrong here. 

It's a crossbeam bounded channel that starts blocking the sender if the thread pool falls behind. 

I thought that this wouldn't be the case because the version on main starts to fail with Disconnected sender when all threads panicked (which drops all channel receivers). Let me revert this change.

---

_@dhruvmanila approved on 2025-05-20 15:00_

> Today, a panic in the worker thread pool gets logged (with tracing) but it tears down the worker thread on which the background task ran.
> The panic will only get surfaced once the thread pool shuts down (when `JoinHandle::join` is called). Eventually, `job_sender.send` panics
> because the thread pool ran out of worker threads and the job queue overflows.

I'm a bit unsure of what this means in practice specifically the "panic will only get surfaced once the thread pool shuts down". I tried adding a panic to the hover handler on main and it does surface the panic in the logs. Or, am I misunderstanding?

> I added a `panic` to the hover request handler and it aborted the process (which VS code then restarts up to 5 times).

I'm still not sure why should we abort the process if there's a panic in a specific handler. Wouldn't that degrade the user experience? Like, today even if there's a panic the server keeps running and users can keep using other capabilities.

Is there a way to handle it gracefully? I might need to spend some time understanding the scheduler but I don't want to block this PR for that. Happy to go ahead with this.

---

_Comment by @MichaReiser on 2025-05-20 15:11_

> I'm a bit unsure of what this means in practice specifically the "panic will only get surfaced once the thread pool shuts down". I tried adding a panic to the hover handler on main and it does surface the panic in the logs. Or, am I misunderstanding?

Thanks to our global panic handler, it does surface the panic in the logs, but it also aborts the thread and we'll eventually run out. The eror value of the panic will not be dropped until we join the threads (which can be problematic if it needs to release any resources). 

For that reason, I think it's the right decision to abort the process.

> I'm still not sure why should we abort the process if there's a panic in a specific handler. Wouldn't that degrade the user experience? Like, today even if there's a panic the server keeps running and users can keep using other capabilities.

Sort of. It works for as long as there are still enough worker threads. Ruff/ty will abort once all threads are used up. But I agree that the experience is worse. I plan to add specific `catch_panic` handlers to the `request` / `notification` handlers which will give us the old behavior (except that we never run out of threads). The last step is then to also implement a retry logic if a thread unwinds due to a `salsa::Cancelled`, which also needs the `catch_unwind` in the `request` handler. 



---

_Comment by @MichaReiser on 2025-05-23 11:58_

This is actually a more sever problem than I thought. Threads panicking has the result that the server never responds to that client request. The client might decide to **not send** any new request for the same method and parameters because there's already a pending request.

I think we should backport my changes to ruff

---

_Comment by @dhruvmanila on 2025-05-26 10:17_

Thank you for the explanation. I think that makes sense and we should do the same for Ruff as well, it might be useful to do that after your planned follow-up work?

---

_Comment by @MichaReiser on 2025-05-26 11:57_

I plan to back port all changes of this stack to ruff

---

_Merged by @MichaReiser on 2025-05-26 12:09_

---

_Closed by @MichaReiser on 2025-05-26 12:09_

---

_Branch deleted on 2025-05-26 12:09_

---

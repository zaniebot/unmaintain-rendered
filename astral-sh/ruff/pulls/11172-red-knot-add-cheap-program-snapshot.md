```yaml
number: 11172
title: "[red-knot] Add \"cheap\" `program.snapshot` "
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-snapshotting
created_at: 2024-04-27T06:48:06Z
updated_at: 2024-04-30T16:08:37Z
url: https://github.com/astral-sh/ruff/pull/11172
synced_at: 2026-01-10T22:37:02Z
```

# [red-knot] Add "cheap" `program.snapshot` 

---

_Pull request opened by @MichaReiser on 2024-04-27 06:48_

## Summary

This PR implements a snapshotting mechanism for `Database` and makes query cancellation a `Database` concept. The motivation for this change is the LSP where the main loop in the LSP uses `spawn` to schedule some work on its thread pool. `sapwn` requires that all arguments have a `'static` lifetime. That means passing a `&Program` as we've done in the CLI main loop won't be possible. 

To solve this, this PR introduces a `program.snapshot` method that returns an owned but read-only database instance. The LSP can safely pass the snapshot to its worker thread because it satisfies the `'static` lifetime requirement. 

The main challenge of the `snapshot` function is that we don't want to create a deep-clone of the database including the `jars` state because:

* a deep-clone of the caches is very expensive
* it would mean that caching is restricted to a single thread. However, we want to share a single cache between all threads.

Getting cheap "clones" is achieved by wrapping the `jars` state in an `Arc`. However, this introduces a new problem. Now, mutating is expensive because `Arc`s are read-only by default. Solving this, requires introducing cancellation.

The implementation makes use of the fact that mutating an `Arc` in place is possible if the `Arc` has exactly one reference. The `Database` now stores its own cancellation token and calling a mutation method on the storage (or database) automatically requests cancellation of all queries and waits until all other references to `jars` are dropped. It is then possible to safely take the mutable reference from the `Arc`.

Waiting is implemented using a [`WaitGroup`](https://docs.rs/crossbeam/latest/crossbeam/sync/struct.WaitGroup.html). The WaitGroup is stored in the `Jars` storage, and its counter is incremented whenever a `Snapshot` is created and automatically decremented when a `Snapshot` drops. 

The last missing piece is to ensure that queries stop "soonish" when cancellation is requested. This is achieved by changing the `db.jar()` method to return a `QueryResult`. It returns `Err(QueryError::Cancelled)` in case cancellation has been requested. This requires that we change the return type of each query to `QueryResult` because they won't have a result when they're cancelled. 
Checking cancellation in the `jars` method has the advantage that it is automatically tested by each query that uses caching because the method is needed to retrieve the query storage. Queries have the possibility to manually test for cancellation if needed by calling `db.cancelled()?`, but that should only be necessary for long operations that never issue a new query. 


## Catch unwind

An alternative to `QueryResult` is to panic with a specific error and catch that error in a catch unwind boundary. This is what salsa does. I decided to use a `Result` instead to make cancellation more explicit. I think that it is important to be aware that a request can be cancelled because it means that we need to ensure to never write "partial" results into the cache, and if we do, have a way to undo the change in case the query gets cancelled to leave the cache in a clean state. 

## Test plan

I ran the linter and used `touch` to trigger a re-run. This PR adds a new `RED_KNOT_SLOW_LINT` that adds an artificial slowdown to `lint_syntax` for testing cancellation. 

## Attribution

The ideas here are heavily inspired by Salsa and sometimes applied 1:1. 



---

_Comment by @github-actions[bot] on 2024-04-28 10:58_

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

_Renamed from "[red-knot] Add snapshotting mechanism" to "[red-knot] Add "cheap" `program.snapshot` " by @MichaReiser on 2024-04-29 09:21_

---

_@MichaReiser reviewed on 2024-04-29 09:22_

---

_Review comment by @MichaReiser on `crates/red_knot/src/cancellation.rs`:12 on 2024-04-29 09:22_

I simplified the implementation because we never used the `wait` method that needs the condvar.

---

_@MichaReiser reviewed on 2024-04-29 10:24_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:140 on 2024-04-29 10:24_

@snowsignal I think the loop here is now very similar to what we have in the LSP. That's why I think that it should now be easy to implement the database into the LSP.

---

_@MichaReiser reviewed on 2024-04-29 10:25_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:236 on 2024-04-29 10:25_

@carljm Thanks for the suggestion using a revision. I think it simplifies a lot. 

---

_@MichaReiser reviewed on 2024-04-29 10:27_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:230 on 2024-04-29 10:27_

This can deadlock when `files.len() > max_concurrency)`. I have a follow up PR to fix this.

---

_@MichaReiser reviewed on 2024-04-29 10:28_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:130 on 2024-04-29 10:28_

I think we can derive the implementations in the future by having a `#[derive(Db(field_name=jars, jars=(SourceJar, SemanticJar)))]` 

---

_Label `internal` added by @MichaReiser on 2024-04-29 10:33_

---

_Review requested from @carljm by @MichaReiser on 2024-04-29 11:08_

---

_Review requested from @snowsignal by @MichaReiser on 2024-04-29 11:08_

---

_Marked ready for review by @MichaReiser on 2024-04-29 11:08_

---

_@MichaReiser reviewed on 2024-04-29 11:09_

---

_Review comment by @MichaReiser on `crates/red_knot/src/db/storage.rs`:65 on 2024-04-29 11:09_

This takes a `&mut self`, so this method can only be called from the main database but never from a `Snapshot`.

---

_Review comment by @snowsignal on `crates/red_knot/src/db.rs`:56 on 2024-04-29 16:40_

I think this sentence is unfinished?

---

_Review comment by @snowsignal on `crates/red_knot/src/db.rs`:96 on 2024-04-29 16:54_

I really like how you ensured snapshot immutability here 😄 

---

_Review comment by @snowsignal on `crates/red_knot/src/main.rs`:140 on 2024-04-29 17:05_

This does look similar! I think the main difference is that here, a response is sent back to the main loop, whereas server tasks don't (currently) communicate with the main loop. That shouldn't make things harder for the server (in fact, I think it will be even easier than what we have here), I just wanted to point that out.

---

_Review comment by @snowsignal on `crates/red_knot/src/program/check.rs`:274 on 2024-04-29 17:18_

Shouldn't we return `Err(QueryError::Cancelled)` immediately if we get `CheckFileMessage::Cancelled`, instead of waiting for the loop to finish?

---

_@snowsignal approved on 2024-04-29 17:23_

Thank you for implementing this! The detailed comments you left made this pull request really easy to read, and I can see a way to integrate this with the server as-is 😄 

I've left a few small comments but otherwise this looks great!

---

_@carljm approved on 2024-04-29 23:47_

Looks good!

To be clear, the "immutable snapshot" means we won't be applying file changes or invalidating anything concurrently (which is great, because it will simplify invalidation a lot). But caching within e.g. `TypeStore` uses interior mutability and can still be updated in workers, because it implements its own internal locking and doesn't rely on having an exclusive reference to the db.

---

_@MichaReiser reviewed on 2024-04-30 06:49_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:140 on 2024-04-30 06:49_

That's true. Thanks for pointing this out. I agree, I don't think this should matter because that communication is only about how results are communicated back to the user interface. In the LSP case, that's done by sending a response and in the CLI case it's done by sending a message back to the main loop.

---

_@MichaReiser reviewed on 2024-04-30 06:55_

---

_Review comment by @MichaReiser on `crates/red_knot/src/db.rs`:96 on 2024-04-30 06:55_

Thank you, but I don't really deserve the credit because I only stole the idea from salsa :D But I agree, it's a very clever (and simple) way. 

---

_@MichaReiser reviewed on 2024-04-30 06:58_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:274 on 2024-04-30 06:58_

The problem with returning immediately is that we then drop the only reference to `receiver`. That means, that any `sender.send(message).unwrap()` calls will fail in the threads checking the files. I'm not sure if there's a more idiomatic way of doing this other than waiting for all file check operations to complete to be sure there will be no more incoming messages and only then exit.

---

_Comment by @MichaReiser on 2024-04-30 07:00_

> To be clear, the "immutable snapshot" means we won't be applying file changes or invalidating anything concurrently (which is great, because it will simplify invalidation a lot). But caching within e.g. TypeStore uses interior mutability and can still be updated in workers, because it implements its own internal locking and doesn't rely on having an exclusive reference to the db.

Yes, that's correct. The immutable snapshot still allows for caches to cache new values, but no "inputs" should change (which would change the result of  the analysis). Another way to think about this. 


---

_Merged by @MichaReiser on 2024-04-30 07:13_

---

_Closed by @MichaReiser on 2024-04-30 07:13_

---

_Branch deleted on 2024-04-30 07:13_

---

_@snowsignal reviewed on 2024-04-30 15:12_

---

_Review comment by @snowsignal on `crates/red_knot/src/program/check.rs`:274 on 2024-04-30 15:12_

Can't we just handling that error in the threads checking the files though? Like, if `sender.send(message)` fails, we can still exit the thread gracefully instead of panicking.

---

_@MichaReiser reviewed on 2024-04-30 16:08_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:274 on 2024-04-30 16:08_

I guess we could, but it would mean that it will be harder to find the bug if we drop the receiver accidentally for another reason. Anyway. The next PR reworked this quiet significantly and I think there we can return immediately.

---

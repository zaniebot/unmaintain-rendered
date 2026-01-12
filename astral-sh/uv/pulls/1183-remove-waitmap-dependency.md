```yaml
number: 1183
title: "Remove `WaitMap` dependency"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/wait
created_at: 2024-01-30T05:02:22Z
updated_at: 2024-01-30T20:25:24Z
url: https://github.com/astral-sh/uv/pull/1183
synced_at: 2026-01-12T16:04:29Z
```

# Remove `WaitMap` dependency

---

_@charliermarsh_

## Summary

This is an attempt to https://github.com/astral-sh/puffin/pull/1163 by removing the `WaitMap` and gaining more granular control over the values that we hold over `await` boundaries.


---

_Review requested from @konstin by @charliermarsh on 2024-01-30 05:02_

---

_Comment by @charliermarsh on 2024-01-30 05:02_

@konstin - I haven't done any benchmarking, but this does consistently not-deadlock for me.

---

_Review comment by @charliermarsh on `crates/once-map/src/lib.rs`:14 on 2024-01-30 05:05_

@BurntSushi might be interested in this as a design problem (but know you have other priorities). This also seems like the perfect kind of work for Ibraheem :)

---

_@charliermarsh reviewed on 2024-01-30 05:05_

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:53 on 2024-01-30 11:34_

Can we assert that on drop, nobody is waiting anymore?

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:409 on 2024-01-30 11:35_

I'd add here that we specifically need this because the channel is sync

---

_Comment by @konstin on 2024-01-30 11:37_

Current dependencies on/for this PR:
* `main`
  * **PR #1183** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1183?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #1163** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1163?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/1183?utm_source=stack-comment).

---

_Comment by @konstin on 2024-01-30 11:39_

(Sorry for pushing this branch, i misread the updated graphite docs)

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:29 on 2024-01-30 11:51_

The docs for [entry](https://docs.rs/dashmap/latest/dashmap/struct.DashMap.html#method.entry) say:

> Locking behaviour: May deadlock if called when holding any sort of reference into the map.

This assumes we never actually call this function from two threads at the same time. This is the same call that `WaitMap` previously deadlocked in. 

If we know that the map will stay on the main thread anyway, we can use `Cell<FxHashMap<K, V>>` instead of `DashMap`. If we want share the index between threads (i don't know that - it depends on how we want to structure our async code) we'll need something like `RwLock<FxHashMap<K, V>>`. `DashMap` is a `Box<[RwLock<HashMap<K, V, S>>]>` internally (https://docs.rs/dashmap/latest/src/dashmap/lib.rs.html#88-92) and our lock times are minimal, so i expect no perf difference. `DashMap` is missing a `get_or_insert() -> bool`, an "atomic" compare-and-swap option, that both we and `WaitMap` would need to be correct.


---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:39 on 2024-01-30 11:55_

Should we also yield here? Notifying is surprisingly sync, so the subscribers won't get notified immediately until the next yielding of the task that called done. (This would make the function async, but i think it's correct that all operations on the `OnceMap` should be async)

---

_@konstin reviewed on 2024-01-30 11:56_

This removes the deadlock! Benchmarks in the upstack PR are looking good. My main concern is deadlock-warning in the `DashMap::entry` api

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:29 on 2024-01-30 13:11_

I don't think the warning is about calling `entry` from two different threads simultaneously. That's normal and should be fine, otherwise it'd be a pretty poor concurrent hashmap. The warning is, AIUI, about calling `entry` (or `get`) while you have a reference to a previous call in hand in the same thread. When multiple threads are calling it, one will (hopefully) eventually make progress, drop the reference and unblock the other thread. But if you do `let entry = self.items.entry(foo);` and then `self.items.get(foo)` while `entry` is still alive, then that `get` call seems likely to block waiting for `entry` to drop. Which, of course, will never happen because the thread is blocked on `get`.

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:30 on 2024-01-30 13:12_

I think you could just call `self.register_owned(key.to_owned())` here?

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:40 on 2024-01-30 13:12_

I'd probably rename this to `register` and drop the existing `register` function. Unless you want the convenience of passing a borrow as a key to this map. (But `register_owned` surfaces the costs better IMO.)

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:39 on 2024-01-30 13:21_

> Notifying is surprisingly sync, so the subscribers won't get notified immediately until the next yielding of the task that called done.

Hmmm. Are you sure? We're using the multi-threaded runtime for tokio right? If so, AIUI, other waiters could be notified and acting on it before `notify.notify_waiters()` even finishes.

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:66 on 2024-01-30 13:23_

I would be tempted to change the signature to `Result<Option<Arc<V>>, Error>` instead. And then callers can `unwrap()` the `Ok` case if they know it's already been inserted.

Or is there a non-bug situation where `Error::NotRegistered` would be returned?

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:53 on 2024-01-30 13:23_

Can you say more?

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:74 on 2024-01-30 13:25_

I think you can `unwrap()` here right? If the `key` is no longer in the map at this point, then that implies a pretty bad bug within the implementation of this `OnceMap`. (Since as far as I can see, callers cannot remove keys using public API of `OnceMap`.)

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:58 on 2024-01-30 13:25_

I buy this.

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:114 on 2024-01-30 13:26_

Is this still used? Do we need cancellation? (I would naively assume "yes," but I'm not 100% sure.)

---

_@BurntSushi reviewed on 2024-01-30 13:31_

This is great. I like the use of `Arc` to simplify things here.

---

_@konstin reviewed on 2024-01-30 13:36_

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:39 on 2024-01-30 13:36_

Yep that's correct, but our receivers are all on the some thread atm as far as i can see.

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:53 on 2024-01-30 13:41_

If there is a task waiting on some request but there is no task providing it (because we're done, we're dropping the once map), this sounds like a bug.

---

_@konstin reviewed on 2024-01-30 13:41_

---

_@konstin reviewed on 2024-01-30 13:42_

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:29 on 2024-01-30 13:42_

Thanks for clarifying, this makes much more sense! I was indeed assuming that dashmap was behaving rather poorly, but this makes more sense and works for us if we don't have any await points while we hold the entry.



---

_Comment by @charliermarsh on 2024-01-30 13:50_

Thank you both so much for the close read, I needed it!

---

_@konstin reviewed on 2024-01-30 13:57_

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:29 on 2024-01-30 13:57_

Thinking about this longer, was the problem with waitmap maybe merely that we there was some yielding while holding an entry?

---

_@charliermarsh reviewed on 2024-01-30 14:00_

---

_Review comment by @charliermarsh on `crates/once-map/src/lib.rs`:29 on 2024-01-30 14:00_

@konstin - I spent a while investigating that, and trying to make changes to the resolver to solve it, but I ultimately couldn't figure out where it might be.

---

_@BurntSushi reviewed on 2024-01-30 14:14_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:53 on 2024-01-30 14:14_

Oh! I see now. Yes. On `Drop` of the `OnceMap`.

I agree it might be a bug... but I could also see that maybe it isn't? Maybe the caller has finished what they needed and didn't need to wait for everything to finish. And it is perhaps hard to distinguish between "there is no task providing it" and "there is a task providing it, but it hasn't done so yet." I defer to y'all here because I don't have enough context on how this is used. But I absolutely agree that if you can assert that any `Waiting` values are a bug, then it might be nice to assert it on `Drop`. (Although note that a panic during `Drop` is an instant abort.)

---

_@BurntSushi reviewed on 2024-01-30 14:15_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:29 on 2024-01-30 14:15_

I also briefly looked at `waitmap`'s implementation and nothing jumped out at me immediately.

---

_Review comment by @zanieb on `crates/once-map/src/lib.rs`:30 on 2024-01-30 15:21_

This could return some `Handle` object that we could use to enforce the invariant, right? e.g. `done` would consume a handle which would have a reference to the key instead and we'd be able to check if the handle was not used on drop.

> If this method returns `true`, you need to start a job and call [`OnceMap::done`] eventually
> or other tasks will hang.

Not sure how problematic that is.

---

_@zanieb reviewed on 2024-01-30 15:21_

---

_@zanieb reviewed on 2024-01-30 15:24_

---

_Review comment by @zanieb on `crates/once-map/src/lib.rs`:39 on 2024-01-30 15:24_

Is there a reason the waiters need to wake up sooner?

---

_@konstin approved on 2024-01-30 16:28_

---

_@konstin reviewed on 2024-01-30 16:42_

---

_Review comment by @konstin on `crates/once-map/src/lib.rs`:39 on 2024-01-30 16:42_

In https://github.com/astral-sh/puffin/pull/1163 i got a speedup from 30ms to 20ms for warm cache jupyter by inserting one `tokio::task::yield_now().await`, now i'm motivated to avoid these kinds of bottlenecks.

---

_@charliermarsh reviewed on 2024-01-30 20:12_

---

_Review comment by @charliermarsh on `crates/once-map/src/lib.rs`:30 on 2024-01-30 20:12_

I agree that would be a much nicer API. Will consider it in a future PR, the dataflow might be tedious for now.

---

_Merged by @charliermarsh on 2024-01-30 20:25_

---

_Closed by @charliermarsh on 2024-01-30 20:25_

---

_Branch deleted on 2024-01-30 20:25_

---

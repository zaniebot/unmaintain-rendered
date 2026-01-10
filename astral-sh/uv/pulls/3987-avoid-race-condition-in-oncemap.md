```yaml
number: 3987
title: "Avoid race condition in `OnceMap`"
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: deadlock
created_at: 2024-06-03T15:23:13Z
updated_at: 2024-06-04T13:39:59Z
url: https://github.com/astral-sh/uv/pull/3987
synced_at: 2026-01-10T13:54:02Z
```

# Avoid race condition in `OnceMap`

---

_Pull request opened by @ibraheemdev on 2024-06-03 15:23_

## Summary

Fixes a race condition in `OnceMap::wait_blocking` where the inserted value could potentially be missed, leading to a deadlock. Fairly certain this will resolve https://github.com/astral-sh/uv/issues/3724.

---

_Review requested from @konstin by @ibraheemdev on 2024-06-03 15:24_

---

_Comment by @charliermarsh on 2024-06-03 15:29_

So the race here is that the value could be filled between the time we fetch it from the map and set up the notifier?

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-03 15:30_

---

_Review requested from @BurntSushi by @konstin on 2024-06-03 15:32_

---

_Comment by @ibraheemdev on 2024-06-03 15:34_

@charliermarsh yes, checking the map again after setting up the notifier is the crucial bit. If the value hasn't been inserted yet, the thread is already in the waiter list ready to be notified. This is a pretty standard algorithm for blocking in a concurrent data-structure (check, register waiter, check again, wait).

---

_@charliermarsh approved on 2024-06-03 15:36_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:49 on 2024-06-03 15:50_

I'm trying to reason through how a deadlock could happen here. My understanding is that `entry` here is actually a [`dashmap::mapref::one::Ref`](https://docs.rs/dashmap/latest/dashmap/mapref/one/struct.Ref.html), and that in turn holds a lock while it's alive. (The `dashmap` docs are woefully incomplete, but it's what the implementation suggests.) If that's true, then once a `get` happens, then it shouldn't be possible for a `done` call to insert anything before the case analysis below, right?

Oh... wait... OK. I now see the `drop(entry)` below just before the `notify.notified().await`. So the deadlock is that between `drop(entry)` and `notify.notified().await`, a `done` call inserts a filled entry for the given key and notifies waiters _before_ `notify.notified()` has a chance to register the waiter.



---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:60 on 2024-06-03 15:52_

Are we sure this is necessary? We are using [`Notify::notify_waiters`](https://docs.rs/tokio/latest/tokio/sync/struct.Notify.html#method.notify_waiters), and it seems like that doesn't benefit from `Notified::enable`, since `notify_waiters` doesn't "store" permits.

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:60 on 2024-06-03 15:53_

That is, I'm wondering if its superfluous (not deleterious).

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:67 on 2024-06-03 15:55_

OK, I buy this, because acquiring the `Notification` has been de-coupled from `await`ing on the `Notification`. And `notify_waiters` specifically says:

> The purpose of this method is to notify all already registered waiters. Registering for notification is done by acquiring an instance of the Notified future via calling notified().

But my reading here is still that `notification.as_mut().enable()` is not needed here. Not unless we're using `notify_{one,last}` somewhere.

---

_@BurntSushi approved on 2024-06-03 15:56_

I buy what you're selling here. Great find. `Notify` is quite a bit more subtle than I had thought.

---

_@ibraheemdev reviewed on 2024-06-03 15:58_

---

_Review comment by @ibraheemdev on `crates/once-map/src/lib.rs`:49 on 2024-06-03 15:58_

> a done call inserts a filled entry for the given key and notifies waiters before notify.notified() has a chance to register the waiter.

Yes, exactly.

---

_@ibraheemdev reviewed on 2024-06-03 16:00_

---

_Review comment by @ibraheemdev on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:00_

It is necessary, calling `notify.notified()` does nothing but construct the future lazily. `enable` is what actually registers the waiter. 

---

_@ibraheemdev reviewed on 2024-06-03 16:02_

---

_Review comment by @ibraheemdev on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:02_

Actually I might be wrong on that, from the docs:
> The Notified future is guaranteed to receive wakeups from notify_waiters() as soon as it has been created, even if it has not yet been polled.

But looking [at the source code](https://docs.rs/tokio/latest/src/tokio/sync/notify.rs.html#531) I'm not sure how that is guaranteed.

---

_@BurntSushi reviewed on 2024-06-03 16:04_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:04_

Yeah but the docs for `Notify::notify_waiters` suggests otherwise. And, specifically, _not_ `Notify::notify_{one,last}`.

---

_@BurntSushi reviewed on 2024-06-03 16:04_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:04_

> The Notified future is guaranteed to receive wakeups from notify_waiters() as soon as it has been created, even if it has not yet been polled.

---

_@BurntSushi reviewed on 2024-06-03 16:07_

---

_Review comment by @BurntSushi on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:07_

Ah whoops, my comments came in after your last two, but I hadn't seen those.

Yeah I'm just going on docs. Not on implementation.

I'd be inclined to leave out superfluous things because it can make things more confusing, but if we don't know it's superfluous we could leave it out and see whether the deadlocks re-appear. Or if you're feeling more conservative, add a comment explaining why it's there even though a read of the docs suggests it isn't needed.

---

_@ibraheemdev reviewed on 2024-06-03 16:08_

---

_Review comment by @ibraheemdev on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:08_

Ah okay, Tokio pulls a little trick here by tracking how many times `notify_waiters` has been called. I missed that, you're correct in that we don't need the `enable`.

---

_@ibraheemdev reviewed on 2024-06-03 16:12_

---

_Review comment by @ibraheemdev on `crates/once-map/src/lib.rs`:60 on 2024-06-03 16:12_

I updated the comments to make it clear this only works with `notify_waiters`.

---

_Comment by @codspeed-hq[bot] on 2024-06-03 16:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:deadlock)

### Merging #3987 will **improve performances by 6.72%**

<sub>Comparing <code>ibraheemdev:deadlock</code> (8ea913b) with <code>main</code> (29ea5d5)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:deadlock` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.7 ns | 868.3 ns | +6.72% |


---

_Merged by @ibraheemdev on 2024-06-03 16:25_

---

_Closed by @ibraheemdev on 2024-06-03 16:25_

---

_Comment by @zanieb on 2024-06-04 13:39_

Awesome!

---

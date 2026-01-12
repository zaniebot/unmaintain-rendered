```yaml
number: 3413
title: "Remove unnecessary uses of `DashMap` and `Arc`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: less-sync
created_at: 2024-05-06T16:52:52Z
updated_at: 2024-05-07T02:30:43Z
url: https://github.com/astral-sh/uv/pull/3413
synced_at: 2026-01-12T16:05:37Z
```

# Remove unnecessary uses of `DashMap` and `Arc`

---

_@ibraheemdev_

## Summary

All of the resolver code is run on the main thread, so a lot of the `Send` bounds and uses of `DashMap` and `Arc` are unnecessary. We could also switch to using single-threaded versions of `Mutex` and `Notify` in some places, but there isn't really a crate that provides those I would be comfortable with using.

The `Arc` in `OnceMap` can't easily be removed because of the uv-auth code which uses the [reqwest-middleware](https://docs.rs/reqwest-middleware/latest/reqwest_middleware/trait.Middleware.html) crate, that seems to adds unnecessary `Send` bounds because of `async-trait`. We could duplicate the code and create a `OnceMapLocal` variant, but I don't feel that's worth it.

---

_@ibraheemdev reviewed on 2024-05-06 16:56_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:144 on 2024-05-06 16:56_

:relieved: 

---

_Comment by @BurntSushi on 2024-05-06 17:00_

> All of the resolver code is run on the main thread, so a lot of the `Send` bounds and uses of `DashMap` and `Arc` are unnecessary.

Is the resolver running on just the main thread what we want? If we wanted it to make use of multiple threads in the future (even as an experiment to try?), then adding these `Send` bounds back could be pretty annoying. (I'm not sure our uses of `DashMap` and `Arc` are costing us much, but maybe I'm wrong about that. Of course, if we don't need them, then I agree with this PR.)

---

_Converted to draft by @ibraheemdev on 2024-05-06 17:02_

---

_Comment by @ibraheemdev on 2024-05-06 17:04_

@BurntSushi That's a good point. I'll mark this as a draft for now before we settle down on our async architecture. I agree we probably don't pay much of a cost for using `DashMap`/`Arc` on a single-thread.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:144 on 2024-05-06 17:11_

Love that!

---

_@charliermarsh reviewed on 2024-05-06 17:11_

---

_Marked ready for review by @ibraheemdev on 2024-05-06 17:28_

---

_Comment by @ibraheemdev on 2024-05-06 17:33_

Actually I'm going to take that back. Making any of the resolver or installer code multi-threaded would be very hard because of the `'a` lifetime that proliferates throughout the codebase. The changes required to do that would be significantly more than reverting this commit. I'd also be surprised to see any gain from making those changes that we couldn't get by optimizing our use of the single threaded scheduler, given our scale of I/O.

---

_@charliermarsh approved on 2024-05-06 17:49_

I'm supportive of the change, though good to get @BurntSushi sign-off too before merging since he chimed in with comments.

---

_Label `internal` added by @charliermarsh on 2024-05-06 17:49_

---

_@BurntSushi approved on 2024-05-06 17:58_

This LGTM. I'm overall in favor of simplifying the code as it exists under current constraints, and if we find ourselves needing to add this (or something like it) back, then it doesn't seem that horrible to do.

---

_Merged by @charliermarsh on 2024-05-07 02:30_

---

_Closed by @charliermarsh on 2024-05-07 02:30_

---

```yaml
number: 3493
title: Consolidate concurrency limits
type: pull_request
state: merged
author: ibraheemdev
labels:
  - configuration
assignees: []
merged: true
base: main
head: concurrency-limits
created_at: 2024-05-09T18:37:12Z
updated_at: 2024-05-10T16:43:09Z
url: https://github.com/astral-sh/uv/pull/3493
synced_at: 2026-01-10T14:37:54Z
```

# Consolidate concurrency limits

---

_Pull request opened by @ibraheemdev on 2024-05-09 18:37_

## Summary

This PR consolidates the concurrency limits used throughout `uv` and exposes two limits, `UV_CONCURRENT_DOWNLOADS` and `UV_CONCURRENT_BUILDS`, as environment variables.

Currently, `uv` has a number of concurrent streams that it buffers using relatively arbitrary limits for backpressure. However, many of these limits are conflated. We run a relatively small number of tasks overall and should start most things as soon as possible. What we really want to limit are three separate operations:
- File I/O. This is managed by tokio's blocking pool and we should not really have to worry about it.
- Network I/O.
- Python build processes.

Because the current limits span a broad range of tasks, it's possible that a limit meant for network I/O is occupied by tasks performing builds, reading from the file system, or even waiting on a `OnceMap`. We also don't limit build processes that end up being required to perform a download. While this may not pose a performance problem because our limits are relatively high, it does mean that the limits do not do what we want, making it tricky to expose them to users (https://github.com/astral-sh/uv/issues/1205, https://github.com/astral-sh/uv/issues/3311).

After this change, the limits on network I/O and build processes are centralized and managed by semaphores. All other tasks are unbuffered (note that these tasks are still bounded, so backpressure should not be a problem).

---

The terminology here is a little weird. From https://github.com/astral-sh/uv/issues/3311, what we want is to expose a way to limit the overall concurrency of downloads. However, for builds, we're really only limiting the number of parallel python processes we spawn. Technically those two are different, but I think sticking to one of concurrency vs. parallelism would be less confusing. Maybe these should be `UV_PARALLEL_*` instead?

## Test Plan

<!-- How was it tested? -->


---

_Comment by @ibraheemdev on 2024-05-09 18:42_

There is one seemingly important limit left in https://github.com/astral-sh/uv/blob/main/crates/uv-resolver/src/resolver/mod.rs#L323, from https://github.com/astral-sh/uv/pull/1163. In practice this bound is quite large compared to our concurrent downloads and should not be a source of contention, it's mainly there to allow us to use a bounded channel and avoid starving the prefetcher.

---

_Comment by @zanieb on 2024-05-09 18:42_

Regarding parallel vs concurrent: I don't think that's important as a user-facing detail. "CONCURRENT" is fine.

---

_Comment by @ibraheemdev on 2024-05-09 18:43_

I agree, I was just wondering which one would be more intuitive, regardless of which is technically correct.

---

_Comment by @zanieb on 2024-05-09 18:53_

👍 I think people have a broader understanding of "concurrency limits" than parallelism.

---

_Comment by @charliermarsh on 2024-05-09 19:53_

I've gotten comfortable just using the word "concurrency" because of this: https://doc.rust-lang.org/book/ch16-00-concurrency.html

---

_@konstin approved on 2024-05-09 20:09_

nice work

---

_@charliermarsh approved on 2024-05-09 20:44_

LGTM! There's maybe one other thing we care about here: the number of concurrent _installs_ (which, today, is controllable by `RAYON_NUM_THREADS`). Once we've downloaded and built all the wheels, we then have to install them into the virtual environment, and we do this with Rayon. I think it's fine _not_ to add a setting for that, but we _could_.

---

_Label `configuration` added by @charliermarsh on 2024-05-09 20:45_

---

_Comment by @zanieb on 2024-05-09 21:56_

I have a minor preference for our own setting over a RAYON variable if it's easy.

---

_Comment by @ibraheemdev on 2024-05-10 16:26_

> LGTM! There's maybe one other thing we care about here: the number of concurrent installs (which, today, is controllable by `RAYON_NUM_THREADS`). Once we've downloaded and built all the wheels, we then have to install them into the virtual environment, and we do this with Rayon. I think it's fine not to add a setting for that, but we could.

We almost might want a way to limit the size of tokio's blocking threadpool for fs operations (https://github.com/astral-sh/uv/issues/3311). Ideally those options could be combined. Installs are fs bound so maybe we should just use tokio's blocking pool instead of rayon for that as well?

Either way I think I'll keep this PR limited to network and build limits.

---

_Merged by @ibraheemdev on 2024-05-10 16:43_

---

_Closed by @ibraheemdev on 2024-05-10 16:43_

---

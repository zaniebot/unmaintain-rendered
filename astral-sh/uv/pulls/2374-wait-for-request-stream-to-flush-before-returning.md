```yaml
number: 2374
title: Wait for request stream to flush before returning resolution
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/join
created_at: 2024-03-12T04:04:24Z
updated_at: 2024-03-12T14:14:24Z
url: https://github.com/astral-sh/uv/pull/2374
synced_at: 2026-01-12T16:05:00Z
```

# Wait for request stream to flush before returning resolution

---

_@charliermarsh_

## Summary

This is a more robust fix for https://github.com/astral-sh/uv/issues/2300.

The basic issue is:

- When we resolve, we attempt to pre-fetch the distribution metadata for candidate packages.
- It's possible that the resolution completes _without_ those pre-fetch responses. (In the linked issue, this was mainly because we were running with `--no-deps`, but the pre-fetch was causing us to attempt to build a package to get its dependencies. The resolution would then finish before the build completed.)
- In that case, the `Index` will be marked as "waiting" for that response -- but it'll never come through.
- If there's a subsequent call to the `Index`, to see if we should fetch or are waiting for that response, we'll end up waiting for it forever, since it _looks_ like it's in-flight (but isn't). (In the linked issue, we had to build the source distribution for the install phase of `pip install`, but `setuptools` was in this bad state from the _resolve_ phase.)

This PR modifies the resolver to ensure that we flush the stream of requests before returning. Specifically, we now `join` rather than `select` between the resolution and request-handling futures.

This _could_ be wasteful, since we don't _need_ those requests, but it at least ensures that every `.wait` is followed by ` .done`. In practice, I expect this not to have any significant effect on performance, since we end up using the pre-fetched distributions almost every time.

## Test Plan

I ran through the test plan from https://github.com/astral-sh/uv/pull/2373, but ran the build 10 times and ensured it never crashed. (I reverted https://github.com/astral-sh/uv/pull/2373, since that _also_ fixes the issue in the proximate case, by never fetching `setuptools` during the resolve phase.)

I also added logging to verify that requests are being handled _after_ the resolution completes, as expected.

I also introduced an arbitrary error in `fetch` to ensure that the error was immediately propagated.


---

_Label `bug` added by @charliermarsh on 2024-03-12 04:04_

---

_Marked ready for review by @charliermarsh on 2024-03-12 04:04_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-12 04:04_

---

_Review requested from @konstin by @charliermarsh on 2024-03-12 04:04_

---

_@charliermarsh reviewed on 2024-03-12 04:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:199 on 2024-03-12 04:04_

Taking ownership means that `request_sink` is dropped as soon as the future completes.

---

_Comment by @charliermarsh on 2024-03-12 04:12_

No meaningful change, as expected:

```
❯ python -m scripts.bench \
        --uv-path ./target/release/uv-main \
        --uv-path ./target/release/uv \
        scripts/requirements/trio.in --benchmark resolve-cold --min-runs 50
Benchmark 1: ./target/release/uv-main (resolve-cold)
  Time (mean ± σ):     245.7 ms ±  48.9 ms    [User: 64.4 ms, System: 62.9 ms]
  Range (min … max):   183.6 ms … 322.8 ms    50 runs

Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     244.8 ms ±  50.2 ms    [User: 64.5 ms, System: 61.7 ms]
  Range (min … max):   179.9 ms … 326.9 ms    50 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.00 ± 0.29 times faster than './target/release/uv-main (resolve-cold)'
```

---

_@zanieb approved on 2024-03-12 05:25_

Nice

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:30 on 2024-03-12 09:36_

I added the panic message to all the broken channel cases because in my experience this error happens when there was a panic in another thread, so the error message you get is ~irrelevant, while scrolling up another thread spilled a first panic to stderr that has the actually relevant failure

---

_@konstin approved on 2024-03-12 09:38_

---

_@BurntSushi approved on 2024-03-12 11:10_

---

_Merged by @charliermarsh on 2024-03-12 14:13_

---

_Closed by @charliermarsh on 2024-03-12 14:13_

---

_Branch deleted on 2024-03-12 14:13_

---

_Comment by @charliermarsh on 2024-03-12 14:14_

I also benchmarked on home-assistant to be safe (this branch is "faster" but I think it's just noise):

```
❯ python -m scripts.bench \
        --uv-path ./target/release/uv-main \
        --uv-path ./target/release/uv \
        scripts/requirements/home-assistant.in --benchmark resolve-cold
Benchmark 1: ./target/release/uv-main (resolve-cold)
  Time (mean ± σ):     14.211 s ±  0.316 s    [User: 66.686 s, System: 29.350 s]
  Range (min … max):   13.691 s … 14.771 s    10 runs

Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     13.791 s ±  0.291 s    [User: 66.304 s, System: 28.658 s]
  Range (min … max):   13.290 s … 14.202 s    10 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.03 ± 0.03 times faster than './target/release/uv-main (resolve-cold)'
```

---

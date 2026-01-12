```yaml
number: 1707
title: re-introduce cache healing when we see an invalid cache entry
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-cache-healing
created_at: 2024-02-19T16:56:34Z
updated_at: 2024-02-23T12:30:16Z
url: https://github.com/astral-sh/uv/pull/1707
synced_at: 2026-01-12T16:04:41Z
```

# re-introduce cache healing when we see an invalid cache entry

---

_@BurntSushi_

This PR introduces more robust cache healing when `uv` fails to
deserialize an existing cache entry.

("Cache healing" in this context means that if `uv` fails to
deserialize a cache entry, then it will automatically invalidate that
entry and re-generate the data. Typically by sending an HTTP request.)

Previous to some optimizations I made around deserialization, we were
already doing this. After those optimizations, deserializing a cache
policy and the payload were split into two steps. While deserializing
a cache policy retained its cache healing behavior, deserializing the
payload did not. This became an issue when #1556 landed, which changed
one of our `rkyv` data types. This in turn made our internal types
incompatible with existing cache entries. One could work-around this
by clearing `uv`'s cache with `uv clean`, but we should just do it
automatically on a cache entry by entry basis.

This does technically introduce a new cost by pessimistically cloning
the HTTP request so that we can re-send it if necessary (see the commit
messages for the knot pushing me toward this approach). So I re-ran my
favorite ad-hoc benchmark:

```
$ hyperfine -w10 --runs 50 "uv-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "uv-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A bart
Benchmark 1: uv-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     114.4 ms ±   3.2 ms    [User: 149.4 ms, System: 221.5 ms]
  Range (min … max):   106.7 ms … 122.0 ms    50 runs

Benchmark 2: uv-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     114.0 ms ±   3.0 ms    [User: 146.0 ms, System: 223.3 ms]
  Range (min … max):   105.3 ms … 121.4 ms    50 runs

Summary
  uv-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.00 ± 0.04 times faster than uv-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

Which is about what I expected.

We should endeavor to have a better testing strategy for these kinds of bugs, but I think it might be a little tricky to do. I created https://github.com/astral-sh/uv/issues/1699 to track that.

Fixes #1571


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-19 16:58_

---

_@zanieb approved on 2024-02-19 16:59_

Looks good to me! Charlie might be a better reviewer though.

---

_Merged by @BurntSushi on 2024-02-19 17:33_

---

_Closed by @BurntSushi on 2024-02-19 17:33_

---

_Branch deleted on 2024-02-19 17:33_

---

_Label `bug` added by @BurntSushi on 2024-02-23 12:30_

---

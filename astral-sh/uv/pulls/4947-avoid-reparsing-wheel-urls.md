```yaml
number: 4947
title: Avoid reparsing wheel URLs
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: ibraheem/fast-lock
created_at: 2024-07-09T22:07:18Z
updated_at: 2024-07-10T09:16:32Z
url: https://github.com/astral-sh/uv/pull/4947
synced_at: 2026-01-12T16:06:32Z
```

# Avoid reparsing wheel URLs

---

_@ibraheemdev_

## Summary

We currently store wheel URLs in an unparsed state because we don't have a stable parsed representation to use with rykv. Unfortunately this means we end up reparsing unnecessarily in a lot of places, especially when constructing a `Lock`. This PR adds a `UrlString` type that lets us avoid reparsing without losing the validity of the `Url`.

## Test Plan

Shaves off another ~10 ms from https://github.com/astral-sh/uv/issues/4860.

```
➜  transformers hyperfine "../../uv/target/profiling/uv lock" "../../uv/target/profiling/baseline lock" --warmup 3
Benchmark 1: ../../uv/target/profiling/uv lock
  Time (mean ± σ):     120.9 ms ±   2.5 ms    [User: 126.0 ms, System: 80.6 ms]
  Range (min … max):   116.8 ms … 125.7 ms    23 runs
 
Benchmark 2: ../../uv/target/profiling/baseline lock
  Time (mean ± σ):     129.9 ms ±   4.2 ms    [User: 127.1 ms, System: 86.1 ms]
  Range (min … max):   123.4 ms … 141.2 ms    23 runs

Summary
  ../../uv/target/profiling/uv lock ran
    1.07 ± 0.04 times faster than ../../uv/target/profiling/baseline lock
```

---

_Label `performance` added by @ibraheemdev on 2024-07-09 22:07_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-09 22:07_

---

_Review requested from @konstin by @ibraheemdev on 2024-07-09 22:07_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:59 on 2024-07-09 22:19_

This looks like added parsing. Is it strictly necessary?

---

_@charliermarsh reviewed on 2024-07-09 22:20_

---

_Review comment by @ibraheemdev on `crates/distribution-types/src/file.rs`:59 on 2024-07-09 22:24_

I'm not 100% sure. I don't think `split_scheme` guarantees that the URLs is necessarily valid, but a `UrlString` should always be a valid `Url`, so I think we need to do this. We might just error a bit earlier than we would before.

---

_@ibraheemdev reviewed on 2024-07-09 22:24_

---

_Comment by @charliermarsh on 2024-07-10 00:59_

This makes sense but what parse calls did we actually remove here, given that `to_url` still has to parse? 

---

_@charliermarsh approved on 2024-07-10 03:09_

---

_Comment by @ibraheemdev on 2024-07-10 04:40_

The win is that `to_url_string` doesn't have to parse, which we can use in `Wheel::from_registry_wheel`.

---

_@konstin approved on 2024-07-10 07:45_

---

_Merged by @ibraheemdev on 2024-07-10 09:16_

---

_Closed by @ibraheemdev on 2024-07-10 09:16_

---

_Branch deleted on 2024-07-10 09:16_

---

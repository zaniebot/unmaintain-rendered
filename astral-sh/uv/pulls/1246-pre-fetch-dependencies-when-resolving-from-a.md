```yaml
number: 1246
title: Pre-fetch dependencies when resolving from a lockfile
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/pre
created_at: 2024-02-04T19:50:13Z
updated_at: 2024-06-11T11:29:34Z
url: https://github.com/astral-sh/uv/pull/1246
synced_at: 2026-01-10T13:54:02Z
```

# Pre-fetch dependencies when resolving from a lockfile

---

_Pull request opened by @charliermarsh on 2024-02-04 19:50_

## Summary

Surprisingly, this makes incremental resolution... consistently slower? So seems like we shouldn't merge this?

```text
❯ python -m scripts.bench --puffin-path ./target/release/main --puffin-path ./target/release/puffin scripts/requirements/home-assistant.in --benchmark resolve-incremental --min-runs 50
Benchmark 1: ./target/release/main (resolve-incremental)
  Time (mean ± σ):     174.8 ms ±   6.3 ms    [User: 270.8 ms, System: 702.4 ms]
  Range (min … max):   158.1 ms … 191.4 ms    50 runs

Benchmark 2: ./target/release/puffin (resolve-incremental)
  Time (mean ± σ):     183.3 ms ±   9.0 ms    [User: 276.1 ms, System: 794.8 ms]
  Range (min … max):   168.2 ms … 217.6 ms    50 runs

Summary
  './target/release/main (resolve-incremental)' ran
    1.05 ± 0.06 times faster than './target/release/puffin (resolve-incremental)'
```

Closes #1206.


---

_Review requested from @konstin by @charliermarsh on 2024-02-04 19:50_

---

_Label `performance` added by @charliermarsh on 2024-02-04 19:50_

---

_Comment by @charliermarsh on 2024-02-04 19:51_

My guess is that this means we end up prioritizing random package metadata requests over distribution metadata requests that are on the critical path for resolution. Perhaps there's a way to do better prioritization on the channels here?

---

_Comment by @charliermarsh on 2024-02-22 17:08_

Closing for now, can always revisit.

---

_Closed by @charliermarsh on 2024-02-22 17:08_

---

_Comment by @ibraheemdev on 2024-06-11 11:29_

Tried this again after the resolver improvements, still seems to be slower:

```
$ python -m scripts.bench \
--uv-path ./target/profiling/uv \
--uv-path ./target/profiling/baseline \
--benchmark resolve-incremental \
./scripts/requirements/home-assistant.in 
Benchmark 1: ./target/profiling/uv (resolve-incremental)
  Time (mean ± σ):     274.7 ms ±   9.3 ms    [User: 211.0 ms, System: 180.7 ms]
  Range (min … max):   263.9 ms … 296.7 ms    11 runs
 
Benchmark 2: ./target/profiling/baseline (resolve-incremental)
  Time (mean ± σ):     263.0 ms ±  16.2 ms    [User: 220.7 ms, System: 185.1 ms]
  Range (min … max):   242.0 ms … 291.2 ms    11 runs
 
Summary
  ./target/profiling/baseline (resolve-incremental) ran
    1.04 ± 0.07 times faster than ./target/profiling/uv (resolve-incremental)
```

Agree the problem is probably around prefetch prioritization.

---

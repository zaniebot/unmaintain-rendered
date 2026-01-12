```yaml
number: 1886
title: Ensure authentication is passed from the index url to distribution files
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/auth-abs
created_at: 2024-02-22T22:19:31Z
updated_at: 2024-02-23T00:10:18Z
url: https://github.com/astral-sh/uv/pull/1886
synced_at: 2026-01-12T16:04:46Z
```

# Ensure authentication is passed from the index url to distribution files

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/1709
Closes https://github.com/astral-sh/uv/issues/1371

Tested with the reproduction provided in #1709 which gets past the HTTP 401.

Reuses the same copying logic we introduced in https://github.com/astral-sh/uv/pull/1874 to ensure authentication is attached to file URLs with a realm that matches that of the index. I had to move the authentication logic into a new crate so it could be used in `distribution-types`.

We will want to something more robust in the future, like track all realms with authentication in a central store and perform lookups there. That's what `pip` does and it allows consolidation of logic like netrc lookups. That refactor feels significant though, and I'd like to get this fixed ASAP so this is a minimal fix.

---

_Label `bug` added by @zanieb on 2024-02-22 22:19_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-22 22:28_

---

_Marked ready for review by @zanieb on 2024-02-22 22:31_

---

_@charliermarsh reviewed on 2024-02-22 23:08_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:61 on 2024-02-22 23:08_

I think we intentionally changed this to use a string rather than a URL because we saw a huge regression due to excessive URL parsing, serialization, and deserialization. We should at least benchmark this since we're now doing an extra URL parse and serialization each time.

---

_@charliermarsh reviewed on 2024-02-22 23:08_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:56 on 2024-02-22 23:08_

Is it possible to skip this work if there's no username or password on the `base`?

---

_@zanieb reviewed on 2024-02-22 23:14_

---

_Review comment by @zanieb on `crates/distribution-types/src/file.rs`:61 on 2024-02-22 23:14_

😬 

---

_@zanieb reviewed on 2024-02-22 23:15_

---

_Review comment by @zanieb on `crates/distribution-types/src/file.rs`:56 on 2024-02-22 23:15_

Yeah we should be able to avoid it in that case

---

_Comment by @zanieb on 2024-02-22 23:27_

Benches show no change

```bash
$ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/baseline \
    ./scripts/requirements/trio.in --benchmark resolve-warm
  
Benchmark 1: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     144.8 ms ±   1.5 ms    [User: 95.8 ms, System: 37.8 ms]
  Range (min … max):   143.4 ms … 149.8 ms    20 runs
 
Benchmark 2: ./target/release/baseline (resolve-warm)
  Time (mean ± σ):     145.0 ms ±   2.6 ms    [User: 95.9 ms, System: 38.1 ms]
  Range (min … max):   142.0 ms … 151.3 ms    20 runs
 
Summary
  ./target/release/uv (resolve-warm) ran
    1.00 ± 0.02 times faster than ./target/release/baseline (resolve-warm)

$ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/baseline \
    ./scripts/requirements/home-assistant.in --benchmark resolve-warm

Benchmark 1: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     302.7 ms ±  10.8 ms    [User: 233.3 ms, System: 326.0 ms]
  Range (min … max):   292.1 ms … 322.9 ms    10 runs
 
Benchmark 2: ./target/release/baseline (resolve-warm)
  Time (mean ± σ):     295.1 ms ±   7.7 ms    [User: 234.0 ms, System: 336.4 ms]
  Range (min … max):   285.1 ms … 312.8 ms    10 runs
 
Summary
  ./target/release/baseline (resolve-warm) ran
    1.03 ± 0.05 times faster than ./target/release/uv (resolve-warm)

$ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/baseline \
    ./scripts/requirements/all-kinds.in --benchmark resolve-warm

Benchmark 1: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     798.0 ms ±  24.9 ms    [User: 306.8 ms, System: 81.8 ms]
  Range (min … max):   776.3 ms … 859.3 ms    10 runs
 
Benchmark 2: ./target/release/baseline (resolve-warm)
  Time (mean ± σ):     807.3 ms ±  55.0 ms    [User: 305.5 ms, System: 84.0 ms]
  Range (min … max):   744.5 ms … 941.5 ms    10 runs
 
Summary
  ./target/release/uv (resolve-warm) ran
    1.01 ± 0.08 times faster than ./target/release/baseline (resolve-warm)

$ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/baseline \
    ./scripts/requirements/all-kinds.in --benchmark resolve-warm

Benchmark 1: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     798.0 ms ±  24.9 ms    [User: 306.8 ms, System: 81.8 ms]
  Range (min … max):   776.3 ms … 859.3 ms    10 runs
 
Benchmark 2: ./target/release/baseline (resolve-warm)
  Time (mean ± σ):     807.3 ms ±  55.0 ms    [User: 305.5 ms, System: 84.0 ms]
  Range (min … max):   744.5 ms … 941.5 ms    10 runs
 
Summary
  ./target/release/uv (resolve-warm) ran
    1.01 ± 0.08 times faster than ./target/release/baseline (resolve-warm)

$ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/baseline \
    ./scripts/requirements/all-kinds.in --benchmark resolve-cold

Benchmark 1: ./target/release/uv (resolve-cold)
  Time (mean ± σ):      1.472 s ±  0.033 s    [User: 1.107 s, System: 0.572 s]
  Range (min … max):    1.427 s …  1.526 s    10 runs
 
Benchmark 2: ./target/release/baseline (resolve-cold)
  Time (mean ± σ):      1.492 s ±  0.124 s    [User: 1.075 s, System: 0.578 s]
  Range (min … max):    1.395 s …  1.838 s    10 runs
 
Summary
  ./target/release/uv (resolve-cold) ran
    1.01 ± 0.09 times faster than ./target/release/baseline (resolve-cold)
```

---

_@zanieb reviewed on 2024-02-22 23:44_

---

_Review comment by @zanieb on `crates/distribution-types/src/file.rs`:61 on 2024-02-22 23:44_

Benches look fine

---

_@zanieb reviewed on 2024-02-22 23:45_

---

_Review comment by @zanieb on `crates/distribution-types/src/file.rs`:56 on 2024-02-22 23:45_

Skipped in https://github.com/astral-sh/uv/pull/1886/commits/eb1d6a611fab61dfeedf462ae1baa3d763cdabdd

No changes to integration benchmarks

---

_@charliermarsh approved on 2024-02-22 23:57_

Rock on

---

_Merged by @zanieb on 2024-02-23 00:10_

---

_Closed by @zanieb on 2024-02-23 00:10_

---

_Branch deleted on 2024-02-23 00:10_

---

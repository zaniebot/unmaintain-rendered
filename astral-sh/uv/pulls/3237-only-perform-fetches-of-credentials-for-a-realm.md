```yaml
number: 3237
title: Only perform fetches of credentials for a realm once
type: pull_request
state: merged
author: zanieb
labels:
  - performance
assignees: []
merged: true
base: main
head: zb/auth-fetch-once
created_at: 2024-04-24T14:06:20Z
updated_at: 2024-04-24T14:53:45Z
url: https://github.com/astral-sh/uv/pull/3237
synced_at: 2026-01-12T16:05:31Z
```

# Only perform fetches of credentials for a realm once

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/3205

Tested with

`RUST_LOG=uv=trace cargo run -- pip install -r scripts/requirements/trio.in --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess -vv --reinstall 2>&1 | grep keyring`

On `main` you can see a dozen keyring attempts at once. Here, the other requests wait for the first attempt and only a single keyring call is performed.

---

_Label `performance` added by @zanieb on 2024-04-24 14:06_

---

_@zanieb reviewed on 2024-04-24 14:09_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:15 on 2024-04-24 14:09_

This duplication hints that we should refactor the `realm` cache but I don't want to make further changes here.

---

_Review requested from @charliermarsh by @zanieb on 2024-04-24 14:09_

---

_Marked ready for review by @zanieb on 2024-04-24 14:09_

---

_Comment by @zanieb on 2024-04-24 14:12_

Going to do some sort of light benchmarking.

---

_@charliermarsh approved on 2024-04-24 14:30_

---

_@charliermarsh reviewed on 2024-04-24 14:31_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:15 on 2024-04-24 14:31_

`pub(crate)` seems slightly off but perhaps not worth exposing these as methods.

---

_Comment by @zanieb on 2024-04-24 14:52_

Looks helpful for large requirements.

Note for all of these I seeded a clean environment first to ensure there wasn't an extra cost for the first invocation.

```
❯ hyperfine -n branch -n main -- "./branch pip install -r ../prefect/requirements.txt --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q" "./main pip install -r ../prefect/requirements.txt --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q"
Benchmark 1: branch
  Time (mean ± σ):     10.714 s ±  3.331 s    [User: 1.595 s, System: 2.350 s]
  Range (min … max):    7.503 s … 15.264 s    10 runs
 
Benchmark 2: main
  Time (mean ± σ):     16.423 s ±  2.307 s    [User: 26.098 s, System: 8.084 s]
  Range (min … max):   13.985 s … 19.846 s    10 runs
 
Summary
  branch ran
    1.53 ± 0.52 times faster than main
    
❯ hyperfine -n branch -n main -- "./branch pip install -r scripts/requirements/trio.in --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q" "./main pip install -r scripts/requirements/trio.in --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q"
Benchmark 1: branch
  Time (mean ± σ):     12.766 s ±  2.964 s    [User: 1.449 s, System: 1.536 s]
  Range (min … max):    8.814 s … 16.260 s    10 runs
 
Benchmark 2: main
  Time (mean ± σ):     13.331 s ±  3.146 s    [User: 8.309 s, System: 3.164 s]
  Range (min … max):    8.575 s … 17.044 s    10 runs
 
Summary
  branch ran
    1.04 ± 0.35 times faster than main

❯  hyperfine -n branch -n main -- "./branch pip install -r scripts/requirements/compiled/jupyter.txt --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q" "./main pip install -r scripts/requirements/compiled/jupyter.txt --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/zb-test-project-421213/pypyi/simple/ --no-cache --keyring-provider subprocess --reinstall -q"
Benchmark 1: branch
  Time (mean ± σ):     13.692 s ±  3.222 s    [User: 2.062 s, System: 2.956 s]
  Range (min … max):    8.369 s … 17.495 s    10 runs
 
Benchmark 2: main
  Time (mean ± σ):     15.738 s ±  1.982 s    [User: 27.028 s, System: 8.893 s]
  Range (min … max):   13.242 s … 19.283 s    10 runs
 
Summary
  branch ran
    1.15 ± 0.31 times faster than main
```

---

_Merged by @zanieb on 2024-04-24 14:53_

---

_Closed by @zanieb on 2024-04-24 14:53_

---

_Branch deleted on 2024-04-24 14:53_

---

```yaml
number: 875
title: "Use `nextest` test runner in CI"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/nextest
created_at: 2024-01-10T21:17:05Z
updated_at: 2024-01-11T20:25:50Z
url: https://github.com/astral-sh/uv/pull/875
synced_at: 2026-01-12T16:04:15Z
```

# Use `nextest` test runner in CI

---

_@zanieb_

Optimizes our test suite in CI:

- ~Uses `mold` as a linker for a 50% compilation improvement~ split out into #887 
- Uses `cargo nextest` to partition test runs into separate parallel jobs
- Splits build and test jobs to avoid recompilation in partitioned test jobs
- Disables `upload-artifact` compression of the built artifacts for a 50% upload time improvement

At https://github.com/astral-sh/puffin/pull/875/commits/5ccb0af3518689fbd64f3e92cd9fa4ca1eadc1e4 (baseline), the `cargo test` job took 5m 9s
At 0dfbddd27545e0664aa4bb312b9d73916c4e48ab(baseline, the `cargo test` job took 4m 37s
As of https://github.com/astral-sh/puffin/pull/875/commits/56ac4e30757ce78f05182b18da979bf842b4cd83, the `cargo build` job takes 1m 52s  and the `cargo test` jobs take a 1m 37s for a total of 3m 29s.
This comes out to a 25-32% total improvement


Tracking individual slow tests in #878 

---

_Comment by @konstin on 2024-01-10 21:18_

I totally want this (i'm using it almost exclusively locally), but will this ensure `--unreferenced reject` and other cargo insta features?

---

_Comment by @zanieb on 2024-01-10 21:19_

We cannot use `--unreferenced reject` with `nextest` (last I checked)

https://github.com/astral-sh/ruff/pull/6979#issuecomment-1697640501

If it's a 2x speedup like I see locally — it's probably worth it anyway.

---

_Comment by @zanieb on 2024-01-10 21:20_

I could explore setting `unreferenced` another way too — my qualms about forking `insta` are minor.

---

_Comment by @charliermarsh on 2024-01-10 21:21_

I'm fine just dropping `--unreferenced reject`. I think we now use inline snapshots everywhere anyway?

---

_Comment by @zanieb on 2024-01-10 21:29_

Hm 4m 22s (insta) vs 4m 6s (nextest) in CI, not motivating. About half that time is compilation.

---

_Comment by @zanieb on 2024-01-10 21:37_

A very impressive repeated 4m 6s when forcing more threads.

---

_Comment by @zanieb on 2024-01-10 23:00_

At https://github.com/astral-sh/puffin/pull/875/commits/5ccb0af3518689fbd64f3e92cd9fa4ca1eadc1e4, the `cargo test` job took 5m 9s
As of https://github.com/astral-sh/puffin/pull/875/commits/56ac4e30757ce78f05182b18da979bf842b4cd83, the `cargo build` job takes 1m 52s  and the `cargo test` jobs take a 1m 37s for a total of 3m 29s
This comes out to a 32% improvement

These numbers are pretty variable depending on the GitHub runners.

---

_Comment by @zanieb on 2024-01-10 23:46_

Significant times remaining:
- Compilation ~60s
- Test runs ~40s
- Build artifact download ~30s
- Build artifact extraction ~17s
- Build artifact compression ~16s

---

_Comment by @zanieb on 2024-01-11 00:23_

Increasing the compression level increases compression time to 45s without decreasing download times

---

_Comment by @zanieb on 2024-01-11 19:06_

After splitting `mold` out:

build 2m 15s and test 1m 41s for 3m 56s total
vs test (main) 4m 56s total
roughly 20% improvement

---

_Comment by @zanieb on 2024-01-11 20:25_

#889 is better.

---

_Closed by @zanieb on 2024-01-11 20:25_

---

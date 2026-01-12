```yaml
number: 8193
title: "chore(uv): more env var mappings"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: more-env-var-const-usage
created_at: 2024-10-15T02:49:10Z
updated_at: 2024-10-15T12:26:06Z
url: https://github.com/astral-sh/uv/pull/8193
synced_at: 2026-01-12T16:08:12Z
```

# chore(uv): more env var mappings

---

_@samypr100_

## Summary

Small follow up to https://github.com/astral-sh/uv/pull/8151

## Test Plan

Existing tests


---

_@zanieb approved on 2024-10-15 02:50_

I'm a little hesitant to add more build dependencies, but 🤷‍♀️ 

---

_Label `internal` added by @zanieb on 2024-10-15 02:50_

---

_Marked ready for review by @samypr100 on 2024-10-15 02:57_

---

_Comment by @konstin on 2024-10-15 07:54_

Perf impact:

main:
```
$ hyperfine --prepare "cargo clean -p uv-cli" "cargo build -p uv-cli"
Benchmark 1: cargo build -p uv-cli
  Time (mean ± σ):      2.096 s ±  0.019 s    [User: 2.851 s, System: 0.343 s]
  Range (min … max):    2.070 s …  2.131 s    10 runs
```

PR:
```
Benchmark 1: cargo build -p uv-cli
  Time (mean ± σ):      2.093 s ±  0.044 s    [User: 2.853 s, System: 0.336 s]
  Range (min … max):    2.056 s …  2.208 s    10 runs
```

```
$ hyperfine --prepare "cargo clean -p uv-cli && cargo clean -p uv-static" "cargo build -p uv-cli"
Benchmark 1: cargo build -p uv-cli
  Time (mean ± σ):      5.169 s ±  0.044 s    [User: 6.628 s, System: 1.756 s]
  Range (min … max):    5.121 s …  5.278 s    10 runs
```

We indeed increase the dep tree, but given that uv-cli already is pretty much at the top of the build DAG, the perf impact is negligible, i.e. we build uv-static while building other workspace crates that uv-cli depends on.

---

_Merged by @charliermarsh on 2024-10-15 11:59_

---

_Closed by @charliermarsh on 2024-10-15 11:59_

---

_Branch deleted on 2024-10-15 12:26_

---

```yaml
number: 819
title: "Use a single `hyperfine` command for each benchmark"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/hyperfine
created_at: 2024-01-06T20:08:56Z
updated_at: 2024-01-06T20:14:24Z
url: https://github.com/astral-sh/uv/pull/819
synced_at: 2026-01-12T16:04:12Z
```

# Use a single `hyperfine` command for each benchmark

---

_@charliermarsh_

## Summary

Refactors the benchmark script such that we use a single `hyperfine` invocation per benchmark, and thus get the comparative summary, which is _way_ nicer:

```
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     410.3 ms ±  19.9 ms    [User: 173.7 ms, System: 1314.5 ms]
  Range (min … max):   389.7 ms … 452.1 ms    10 runs

Benchmark 2: ./target/release/baseline (install-cold)
  Time (mean ± σ):     418.2 ms ±  14.4 ms    [User: 210.7 ms, System: 1246.0 ms]
  Range (min … max):   397.3 ms … 445.7 ms    10 runs

Summary
  './target/release/puffin (install-cold)' ran
    1.02 ± 0.06 times faster than './target/release/baseline (install-cold)'
```

---

_Marked ready for review by @charliermarsh on 2024-01-06 20:08_

---

_Label `internal` added by @charliermarsh on 2024-01-06 20:09_

---

_Merged by @charliermarsh on 2024-01-06 20:14_

---

_Closed by @charliermarsh on 2024-01-06 20:14_

---

_Branch deleted on 2024-01-06 20:14_

---

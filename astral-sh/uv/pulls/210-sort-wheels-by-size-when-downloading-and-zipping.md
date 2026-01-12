```yaml
number: 210
title: Sort wheels by size when downloading and zipping
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sort
created_at: 2023-10-26T20:44:28Z
updated_at: 2023-10-26T20:50:57Z
url: https://github.com/astral-sh/uv/pull/210
synced_at: 2026-01-12T16:03:48Z
```

# Sort wheels by size when downloading and zipping

---

_@charliermarsh_

I just learned about this from PackagingCon, and locally, it shows a nice speedup:

```
❯ hyperfine --warmup 3 --prepare "rm -rf .venv && ./target/release/puffin venv .venv" "./target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache" "./target/release/main pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache"
Benchmark 1: ./target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache
  Time (mean ± σ):      3.958 s ±  0.250 s    [User: 1.323 s, System: 5.840 s]
  Range (min … max):    3.652 s …  4.402 s    10 runs

Benchmark 2: ./target/release/main pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache
  Time (mean ± σ):      4.214 s ±  0.451 s    [User: 1.322 s, System: 5.976 s]
  Range (min … max):    3.708 s …  5.268 s    10 runs

Summary
  './target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache' ran
    1.06 ± 0.13 times faster than './target/release/main pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache'
```

---

_Label `performance` added by @charliermarsh on 2023-10-26 20:44_

---

_Merged by @charliermarsh on 2023-10-26 20:50_

---

_Closed by @charliermarsh on 2023-10-26 20:50_

---

_Branch deleted on 2023-10-26 20:50_

---

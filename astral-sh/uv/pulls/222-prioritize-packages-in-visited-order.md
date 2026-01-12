```yaml
number: 222
title: Prioritize packages in visited order
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/priority
created_at: 2023-10-30T00:07:44Z
updated_at: 2023-10-30T00:48:37Z
url: https://github.com/astral-sh/uv/pull/222
synced_at: 2026-01-12T16:03:48Z
```

# Prioritize packages in visited order

---

_@charliermarsh_

This has a pretty significant impact on large resolutions:

```
❯ ./scripts/benchmarks/compile.sh ./scripts/benchmarks/requirements.in
+ TARGET=./scripts/benchmarks/requirements.in
+ hyperfine --runs 20 --warmup 3 --prepare 'rm -f /tmp/requirements.txt' './target/release/puffin --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' './target/release/main --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
Benchmark 1: ./target/release/puffin --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):      3.069 s ±  0.338 s    [User: 0.738 s, System: 0.132 s]
  Range (min … max):    2.800 s …  4.315 s    20 runs

Benchmark 2: ./target/release/main --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):      4.061 s ±  0.180 s    [User: 0.770 s, System: 0.127 s]
  Range (min … max):    3.801 s …  4.508 s    20 runs

Summary
  './target/release/puffin --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' ran
    1.32 ± 0.16 times faster than './target/release/main --no-cache pip-compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
```

---

_Label `performance` added by @charliermarsh on 2023-10-30 00:47_

---

_Merged by @charliermarsh on 2023-10-30 00:48_

---

_Closed by @charliermarsh on 2023-10-30 00:48_

---

_Branch deleted on 2023-10-30 00:48_

---

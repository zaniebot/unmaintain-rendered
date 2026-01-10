---
number: 98
title: Improve efficiency of PubGrub prototype
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-16T02:10:10Z
updated_at: 2023-10-24T19:32:59Z
url: https://github.com/astral-sh/uv/issues/98
synced_at: 2026-01-10T01:23:03Z
---

# Improve efficiency of PubGrub prototype

---

_Issue opened by @charliermarsh on 2023-10-16 02:10_

Can we make it as efficient as the naive solver? (In other words, using PubGrub should be zero cost.)

---

_Comment by @charliermarsh on 2023-10-17 15:52_

The current resolver is about 25% slower than the naive version that it replaced.

Given a trivial set of requirements:

```text
❯ ./scripts/benchmarks/compile.sh ./scripts/benchmarks/requirements.in
+ TARGET=./scripts/benchmarks/requirements.in
+ hyperfine --runs 20 --warmup 3 --prepare 'rm -f /tmp/requirements.txt' './target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' './target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
Benchmark 1: ./target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     407.5 ms ±  16.6 ms    [User: 64.4 ms, System: 18.9 ms]
  Range (min … max):   382.2 ms … 439.0 ms    20 runs

Benchmark 2: ./target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     327.1 ms ±   9.5 ms    [User: 51.3 ms, System: 16.7 ms]
  Range (min … max):   309.8 ms … 348.1 ms    20 runs

Summary
  './target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' ran
    1.25 ± 0.06 times faster than './target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
+ hyperfine --runs 20 --warmup 3 --prepare 'rm -f /tmp/requirements.txt' './target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' './target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
Benchmark 1: ./target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):      34.5 ms ±   0.4 ms    [User: 28.5 ms, System: 5.9 ms]
  Range (min … max):    34.0 ms …  35.4 ms    20 runs

Benchmark 2: ./target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):      26.9 ms ±   0.6 ms    [User: 21.1 ms, System: 6.1 ms]
  Range (min … max):    25.9 ms …  28.4 ms    20 runs

Summary
  './target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' ran
    1.28 ± 0.03 times faster than './target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
```

---

_Comment by @charliermarsh on 2023-10-17 15:55_

With a slightly more extensive lockfile, the gap is a bit larger:

```text
❯ ./scripts/benchmarks/compile.sh ./scripts/benchmarks/requirements.in
+ TARGET=./scripts/benchmarks/requirements.in
+ hyperfine --runs 20 --warmup 3 --prepare 'rm -f /tmp/requirements.txt' './target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' './target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
Benchmark 1: ./target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     774.1 ms ±  31.2 ms    [User: 328.4 ms, System: 58.2 ms]
  Range (min … max):   714.9 ms … 833.2 ms    20 runs

Benchmark 2: ./target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     570.7 ms ±  26.7 ms    [User: 240.8 ms, System: 49.4 ms]
  Range (min … max):   526.7 ms … 622.0 ms    20 runs

Summary
  './target/release/naive --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' ran
    1.36 ± 0.08 times faster than './target/release/puffin --no-cache compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
+ hyperfine --runs 20 --warmup 3 --prepare 'rm -f /tmp/requirements.txt' './target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' './target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
Benchmark 1: ./target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     262.7 ms ±   1.3 ms    [User: 249.6 ms, System: 33.1 ms]
  Range (min … max):   261.3 ms … 265.9 ms    20 runs

Benchmark 2: ./target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt
  Time (mean ± σ):     184.7 ms ±   2.7 ms    [User: 172.7 ms, System: 28.3 ms]
  Range (min … max):   181.5 ms … 190.7 ms    20 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/naive compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt' ran
    1.42 ± 0.02 times faster than './target/release/puffin compile ./scripts/benchmarks/requirements.in > /tmp/requirements.txt'
```

---

_Comment by @charliermarsh on 2023-10-17 15:56_

In the case that we don't do any backtracking, we should strive to have the same performance (if possible) as the naive resolver.

---

_Comment by @charliermarsh on 2023-10-17 20:20_

I think the entire difference in the simple cached case is actually due to #109 which causes us to do _more_ work, since we iterate over all wheels even if we never needed to see them.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Comment by @charliermarsh on 2023-10-24 19:32_

Closing this for now as not actionable.

---

_Closed by @charliermarsh on 2023-10-24 19:32_

---

```yaml
number: 84
title: Parallelize wheel installations with Rayon
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/parallel-install
created_at: 2023-10-10T19:21:25Z
updated_at: 2023-10-11T03:46:31Z
url: https://github.com/astral-sh/uv/pull/84
synced_at: 2026-01-12T16:03:44Z
```

# Parallelize wheel installations with Rayon

---

_@charliermarsh_

It looks like using _either_ async Rust with a `JoinSet` _or_ parallelizing a fixed threadpool with Rayon provide about a ~5% speed-up over our current serial approach:

```console
❯ hyperfine --runs 30 --warmup 5 --prepare "./target/release/puffin venv .venv" \
  "./target/release/rayon sync ./scripts/benchmarks/requirements-large.txt" \
  "./target/release/async sync ./scripts/benchmarks/requirements-large.txt" \
  "./target/release/main sync ./scripts/benchmarks/requirements-large.txt"
Benchmark 1: ./target/release/rayon sync ./scripts/benchmarks/requirements-large.txt
  Time (mean ± σ):     295.7 ms ±  16.9 ms    [User: 28.6 ms, System: 263.3 ms]
  Range (min … max):   249.2 ms … 315.9 ms    30 runs

Benchmark 2: ./target/release/async sync ./scripts/benchmarks/requirements-large.txt
  Time (mean ± σ):     296.2 ms ±  20.2 ms    [User: 36.1 ms, System: 340.1 ms]
  Range (min … max):   258.0 ms … 359.4 ms    30 runs

Benchmark 3: ./target/release/main sync ./scripts/benchmarks/requirements-large.txt
  Time (mean ± σ):     306.6 ms ±  19.5 ms    [User: 25.3 ms, System: 220.5 ms]
  Range (min … max):   269.6 ms … 332.2 ms    30 runs

Summary
  './target/release/rayon sync ./scripts/benchmarks/requirements-large.txt' ran
    1.00 ± 0.09 times faster than './target/release/async sync ./scripts/benchmarks/requirements-large.txt'
    1.04 ± 0.09 times faster than './target/release/main sync ./scripts/benchmarks/requirements-large.txt'
```

It's much easier to just parallelize with Rayon and avoid async in the underlying wheel code, so this PR takes that approach for now.


---

_Merged by @charliermarsh on 2023-10-11 03:46_

---

_Closed by @charliermarsh on 2023-10-11 03:46_

---

_Branch deleted on 2023-10-11 03:46_

---

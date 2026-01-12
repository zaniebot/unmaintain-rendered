```yaml
number: 3751
title: Improve performance of statistics
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: perf-statistics
created_at: 2023-03-26T21:08:32Z
updated_at: 2023-03-26T22:56:45Z
url: https://github.com/astral-sh/ruff/pull/3751
synced_at: 2026-01-12T15:55:13Z
```

# Improve performance of statistics

---

_@JonathanPlasse_

- Close #3750

Running with `--statistics` is now faster than without it.

```console
❯ hyperfine 'cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true'
Benchmark 1: cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true
  Time (mean ± σ):     749.5 ms ±  17.1 ms    [User: 4181.3 ms, System: 100.7 ms]
  Range (min … max):   721.1 ms … 774.2 ms    10 runs
 
❯ hyperfine 'cargo run -p ruff_cli -- check --no-cache --select=ALL . || true'
Benchmark 1: cargo run -p ruff_cli -- check --no-cache --select=ALL . || true
  Time (mean ± σ):     792.6 ms ±  16.4 ms    [User: 4219.7 ms, System: 94.8 ms]
  Range (min … max):   772.9 ms … 829.8 ms    10 runs
```

---

_@charliermarsh reviewed on 2023-03-26 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:480 on 2023-03-26 21:14_

Is it necessary to clone `body` here, and later on too? Not a huge deal, but it seems wasteful given that we only end up using the first `body` anyway.

---

_Comment by @github-actions[bot] on 2023-03-26 21:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.00     14.2±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.4 MB/sec    1.00      3.7±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.9±1.95µs     6.8 MB/sec    1.01    434.1±1.84µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.01      6.3±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.01ms     5.3 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1692.7±3.85µs     9.8 MB/sec    1.00   1678.0±3.63µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.7±0.32µs    16.8 MB/sec    1.00    175.3±0.56µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     19.8±0.77ms     2.1 MB/sec    1.00     19.0±0.86ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.4±0.29ms     3.1 MB/sec    1.00      5.1±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   687.3±37.48µs     4.3 MB/sec    1.00   689.9±56.83µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.05      9.1±0.34ms     2.8 MB/sec    1.00      8.6±0.44ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03     11.0±0.53ms     3.7 MB/sec    1.00     10.6±0.37ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.4±0.10ms     7.0 MB/sec    1.00      2.4±0.10ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.03   276.6±17.99µs    10.7 MB/sec    1.00   269.6±27.33µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.0±0.27ms     5.1 MB/sec    1.00      5.0±0.30ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/printer.rs`:480 on 2023-03-26 21:53_

I tried sorting and iterating over `Message` instead, but it is slower by more than 2x.
You can go ahead and try to optimize if you find something better.

---

_@JonathanPlasse reviewed on 2023-03-26 21:53_

---

_@charliermarsh reviewed on 2023-03-26 22:04_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:480 on 2023-03-26 22:04_

All good, maybe I'll play around with the ownership but this is clearly a huge improvement!

---

_@charliermarsh reviewed on 2023-03-26 22:15_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:480 on 2023-03-26 22:15_

Do you mind testing the version I just pushed?

---

_Comment by @JonathanPlasse on 2023-03-26 22:30_

```console
# Before
❯ hyperfine 'cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true'
Benchmark 1: cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true
  Time (mean ± σ):     752.7 ms ±  10.7 ms    [User: 4186.4 ms, System: 100.1 ms]
  Range (min … max):   736.7 ms … 769.4 ms    10 runs

# After
❯ hyperfine 'cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true'
Benchmark 1: cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL . || true
  Time (mean ± σ):     739.4 ms ±  14.6 ms    [User: 4179.7 ms, System: 92.5 ms]
  Range (min … max):   723.9 ms … 763.4 ms    10 runs
```

---

_Merged by @charliermarsh on 2023-03-26 22:46_

---

_Closed by @charliermarsh on 2023-03-26 22:46_

---

_Branch deleted on 2023-03-26 22:56_

---

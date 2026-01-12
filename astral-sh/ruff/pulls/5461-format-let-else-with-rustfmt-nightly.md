```yaml
number: 5461
title: Format let-else with rustfmt nightly
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: rustfmt-let-else
created_at: 2023-07-02T06:32:13Z
updated_at: 2023-07-03T02:37:13Z
url: https://github.com/astral-sh/ruff/pull/5461
synced_at: 2026-01-12T03:36:55Z
```

# Format let-else with rustfmt nightly

---

_Pull request opened by @andersk on 2023-07-02 06:32_

Support for `let…else` formatting was just merged to nightly (rust-lang/rust#113225). Rerun `cargo fmt` with Rust nightly 2023-07-02 to pick this up. Followup to #939.

---

_Review requested from @dhruvmanila by @andersk on 2023-07-02 06:32_

---

_Comment by @github-actions[bot] on 2023-07-02 06:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.12ms     4.4 MB/sec    1.00      9.2±0.17ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.3 MB/sec    1.00      2.0±0.03ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    222.6±3.62µs    13.3 MB/sec    1.00    223.3±3.07µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.12ms     5.9 MB/sec    1.01      4.3±0.05ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.22ms     2.6 MB/sec    1.00     15.6±0.34ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.07ms     4.4 MB/sec    1.03      3.9±0.05ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   484.1±11.39µs     6.1 MB/sec    1.01   490.9±11.86µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.10ms     5.2 MB/sec    1.00      7.8±0.10ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1661.7±25.12µs    10.0 MB/sec    1.04  1721.3±31.93µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.5±3.64µs    15.7 MB/sec    1.02    192.7±1.01µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.04ms     7.3 MB/sec    1.01      3.5±0.05ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.07ms     4.4 MB/sec    1.10     10.3±0.08ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1974.2±15.96µs     8.4 MB/sec    1.06      2.1±0.01ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    217.8±3.18µs    13.5 MB/sec    1.04    227.4±6.31µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.03ms     5.7 MB/sec    1.06      4.7±0.02ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.15ms     2.6 MB/sec    1.04     16.3±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.08      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.2±4.69µs     7.1 MB/sec    1.06    441.2±7.26µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.09      7.5±0.03ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.05      8.5±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1666.0±11.76µs    10.0 MB/sec    1.02  1704.8±17.94µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.8±1.31µs    16.9 MB/sec    1.03    179.9±1.74µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     7.0 MB/sec    1.01      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-03 02:13_

---

_Closed by @charliermarsh on 2023-07-03 02:13_

---

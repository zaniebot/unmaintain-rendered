```yaml
number: 4931
title: Drop libafl to fix the cargo-fuzz CI
type: pull_request
state: closed
author: addisoncrump
labels: []
assignees: []
base: cargo_fuzz_install
head: cargo_fuzz_install
created_at: 2023-06-07T15:34:28Z
updated_at: 2023-06-07T16:04:54Z
url: https://github.com/astral-sh/ruff/pull/4931
synced_at: 2026-01-12T15:55:17Z
```

# Drop libafl to fix the cargo-fuzz CI

---

_@addisoncrump_

See https://github.com/charliermarsh/ruff/pull/4928#issuecomment-1581065120.

---

_Comment by @github-actions[bot] on 2023-06-07 15:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      6.5±0.25ms     6.3 MB/sec     1.00      6.4±0.26ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1286.1±53.93µs    12.9 MB/sec     1.01  1298.8±60.90µs    12.8 MB/sec
formatter/numpy/globals.py                 1.00    144.9±8.81µs    20.4 MB/sec     1.02    147.4±9.27µs    20.0 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.17ms     8.9 MB/sec     1.00      2.9±0.13ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.01     17.0±0.48ms     2.4 MB/sec     1.00     16.9±0.59ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.13ms     4.1 MB/sec     1.00      4.0±0.14ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   500.6±22.58µs     5.9 MB/sec     1.01   504.8±26.69µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.23ms     3.6 MB/sec     1.02      7.2±0.30ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.25ms     5.1 MB/sec     1.00      7.9±0.24ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.8±109.74µs     9.7 MB/sec    1.00  1713.0±80.06µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   202.1±15.89µs    14.6 MB/sec     1.00    201.8±9.95µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.09ms     7.2 MB/sec     1.03      3.7±0.18ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11      7.4±0.09ms     5.5 MB/sec    1.00      6.7±0.07ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.09  1397.7±26.59µs    11.9 MB/sec    1.00  1283.4±21.57µs    13.0 MB/sec
formatter/numpy/globals.py                 1.06    150.0±4.37µs    19.7 MB/sec    1.00    141.7±4.10µs    20.8 MB/sec
formatter/pydantic/types.py                1.10      3.2±0.05ms     8.0 MB/sec    1.00      2.9±0.04ms     8.8 MB/sec
linter/all-rules/large/dataset.py          1.01     17.2±0.20ms     2.4 MB/sec    1.00     17.1±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.5±9.10µs     5.9 MB/sec    1.00    492.7±6.68µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.12ms     3.5 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.08ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1759.8±17.21µs     9.5 MB/sec    1.00  1738.1±18.25µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.4±3.65µs    15.0 MB/sec    1.00    197.0±4.13µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.05ms     6.8 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @konstin on 2023-06-07 15:57_

---

_Comment by @konstin on 2023-06-07 16:04_

Sorry, i didn't realize you chose my branch as base branch! could you reopen this against main?

---

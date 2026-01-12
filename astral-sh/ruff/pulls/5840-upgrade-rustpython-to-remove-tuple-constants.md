```yaml
number: 5840
title: upgrade rustpython to remove tuple-constants
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: rm-tuple-constant
created_at: 2023-07-17T20:55:57Z
updated_at: 2023-07-17T23:06:52Z
url: https://github.com/astral-sh/ruff/pull/5840
synced_at: 2026-01-12T03:30:21Z
```

# upgrade rustpython to remove tuple-constants

---

_Pull request opened by @davidszotten on 2023-07-17 20:55_

c.f. https://github.com/astral-sh/RustPython-Parser/pull/28

Tests: No snapshots changed

---

_Comment by @charliermarsh on 2023-07-17 21:21_

This LGTM, the cargo-fuzz failure is confusing.

---

_Comment by @github-actions[bot] on 2023-07-17 21:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.05ms     4.3 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1861.4±3.09µs     8.9 MB/sec    1.00   1840.1±2.39µs     9.0 MB/sec
formatter/numpy/globals.py                 1.02    207.4±0.41µs    14.2 MB/sec    1.00    204.1±0.29µs    14.5 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.02     13.3±0.09ms     3.1 MB/sec    1.00     13.1±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.9 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    441.7±0.75µs     6.7 MB/sec    1.00    433.6±0.97µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.6±0.01ms     6.1 MB/sec    1.00      6.5±0.04ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1419.2±2.24µs    11.7 MB/sec    1.00   1390.3±3.38µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    156.5±0.70µs    18.9 MB/sec    1.00    153.1±0.49µs    19.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.01ms     8.5 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.09ms     3.7 MB/sec    1.00     10.9±0.08ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.1±5.89µs    12.2 MB/sec    1.01    245.0±5.94µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.01      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.14ms     2.6 MB/sec    1.02     15.6±0.19ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.1 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.0±6.74µs     6.0 MB/sec    1.00   490.6±13.34µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.01      7.0±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.06ms     5.2 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1645.7±26.56µs    10.1 MB/sec    1.00  1646.1±16.92µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.1±5.80µs    15.6 MB/sec    1.00    188.7±2.36µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.3 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @zanieb by @charliermarsh on 2023-07-17 21:29_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-17 21:29_

---

_Comment by @zanieb on 2023-07-17 22:37_

@charliermarsh the fuzzer requires a version update too https://github.com/astral-sh/ruff/blob/e008f1790de708e0edd007c6c2fce0483a7f61a0/fuzz/Cargo.toml#L28

---

_Merged by @zanieb on 2023-07-17 22:50_

---

_Closed by @zanieb on 2023-07-17 22:50_

---

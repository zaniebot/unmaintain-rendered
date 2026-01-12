```yaml
number: 6865
title: fix is_raw_string for multiple prefixes
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: fix-is-raw-string
created_at: 2023-08-25T07:26:11Z
updated_at: 2023-08-25T07:58:27Z
url: https://github.com/astral-sh/ruff/pull/6865
synced_at: 2026-01-12T02:45:38Z
```

# fix is_raw_string for multiple prefixes

---

_Pull request opened by @davidszotten on 2023-08-25 07:26_

fix `is_raw_string` in the presence of other prefixes (like `rb"foo"`)

fixes #6864

---

_Comment by @github-actions[bot] on 2023-08-25 07:57_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      4.0±0.34ms    10.1 MB/sec    1.00      3.8±0.17ms    10.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   843.2±39.09µs    19.7 MB/sec    1.05   884.4±76.02µs    18.8 MB/sec
formatter/numpy/globals.py                 1.09     80.8±7.14µs    36.5 MB/sec    1.00     74.4±3.87µs    39.6 MB/sec
formatter/pydantic/types.py                1.03  1722.6±92.39µs    14.8 MB/sec    1.00  1668.4±115.49µs    15.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.9±0.69ms     3.7 MB/sec    1.01     11.0±0.76ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.0±0.22ms     5.6 MB/sec    1.00      2.9±0.13ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.03   425.6±22.48µs     6.9 MB/sec    1.00   411.6±23.08µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.41ms     4.3 MB/sec    1.00      5.8±0.35ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.23ms     7.3 MB/sec    1.00      5.6±0.29ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1202.5±53.09µs    13.8 MB/sec    1.10  1323.2±119.54µs    12.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.0±8.91µs    19.8 MB/sec    1.00    148.6±6.43µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.17ms     9.6 MB/sec    1.00      2.6±0.19ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.7±0.07ms     8.6 MB/sec    1.00      4.7±0.05ms     8.7 MB/sec
formatter/numpy/ctypeslib.py               1.01   970.3±23.62µs    17.2 MB/sec    1.00   964.5±15.70µs    17.3 MB/sec
formatter/numpy/globals.py                 1.00     93.2±2.47µs    31.6 MB/sec    1.02     95.1±3.68µs    31.0 MB/sec
formatter/pydantic/types.py                1.00  1929.0±41.69µs    13.2 MB/sec    1.00  1924.7±24.20µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.02     12.6±0.17ms     3.2 MB/sec    1.00     12.3±0.09ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.04ms     4.8 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.1±8.78µs     6.8 MB/sec    1.00    429.1±8.40µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.06ms     3.9 MB/sec    1.00      6.4±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.09ms     5.8 MB/sec    1.00      6.9±0.09ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1480.2±35.57µs    11.2 MB/sec    1.00  1468.8±22.64µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    174.4±5.19µs    16.9 MB/sec    1.00    173.4±2.48µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-25 07:58_

good catch

---

_Merged by @konstin on 2023-08-25 07:58_

---

_Closed by @konstin on 2023-08-25 07:58_

---

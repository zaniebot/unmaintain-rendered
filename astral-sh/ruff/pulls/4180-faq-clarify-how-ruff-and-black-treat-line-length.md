```yaml
number: 4180
title: "faq: Clarify how Ruff and Black treat line-length."
type: pull_request
state: merged
author: cclauss
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-05-02T08:09:22Z
updated_at: 2023-05-02T23:44:14Z
url: https://github.com/astral-sh/ruff/pull/4180
synced_at: 2026-01-12T15:55:14Z
```

# faq: Clarify how Ruff and Black treat line-length.

---

_@cclauss_

As discussed at https://github.com/micropython/micropython/pull/10977#discussion_r1182175664

---

_Comment by @github-actions[bot] on 2023-05-02 08:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.05ms     2.5 MB/sec    1.00     16.7±0.04ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.14ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.8±2.04µs     5.9 MB/sec    1.00    498.6±0.54µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.01      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1786.3±6.73µs     9.3 MB/sec    1.00   1782.1±3.71µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.6±0.43µs    14.9 MB/sec    1.00    199.5±0.53µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.8±0.11ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.6±0.01ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1276.9±2.28µs    13.0 MB/sec    1.00   1274.6±1.63µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    129.7±0.40µs    22.8 MB/sec    1.00    129.5±0.91µs    22.8 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.41ms     2.4 MB/sec    1.00     16.9±0.46ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.11ms     4.0 MB/sec    1.06      4.5±0.31ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   497.2±13.08µs     5.9 MB/sec    1.00   495.9±10.08µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.22ms     3.6 MB/sec    1.01      7.1±0.27ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.18ms     4.8 MB/sec    1.00      8.5±0.19ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1807.7±41.17µs     9.2 MB/sec    1.00  1802.2±38.48µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.2±5.23µs    14.8 MB/sec    1.02    203.0±8.51µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.7 MB/sec    1.00      3.8±0.09ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.1 MB/sec    1.02      6.8±0.08ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1280.5±16.76µs    13.0 MB/sec    1.02  1306.3±26.33µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    132.3±2.15µs    22.3 MB/sec    1.01    133.9±2.76µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.03      2.9±0.06ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-02 09:45_

---

_Merged by @charliermarsh on 2023-05-02 23:19_

---

_Closed by @charliermarsh on 2023-05-02 23:19_

---

_Branch deleted on 2023-05-02 23:26_

---

```yaml
number: 6624
title: Remove unused runtime string formatting logic
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: internal/remove-fmt
created_at: 2023-08-16T17:22:07Z
updated_at: 2023-08-16T17:54:34Z
url: https://github.com/astral-sh/ruff/pull/6624
synced_at: 2026-01-12T15:55:22Z
```

# Remove unused runtime string formatting logic

---

_@zanieb_

In https://github.com/astral-sh/ruff/pull/6616 we are adding support for nested replacements in format specifiers which makes actually formatting strings infeasible without a great deal of complexity. Since we're not using these functions (they just exist for runtime use in RustPython), we can just remove them.

---

_@charliermarsh approved on 2023-08-16 17:26_

---

_Merged by @zanieb on 2023-08-16 17:38_

---

_Closed by @zanieb on 2023-08-16 17:38_

---

_Branch deleted on 2023-08-16 17:38_

---

_Comment by @github-actions[bot] on 2023-08-16 17:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.04ms    11.3 MB/sec    1.00      3.6±0.03ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    738.2±9.19µs    22.6 MB/sec    1.00    736.0±3.50µs    22.6 MB/sec
formatter/numpy/globals.py                 1.00     76.4±0.33µs    38.6 MB/sec    1.01     76.8±0.33µs    38.4 MB/sec
formatter/pydantic/types.py                1.01  1467.2±36.10µs    17.4 MB/sec    1.00  1457.3±14.50µs    17.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.07ms     3.9 MB/sec    1.01     10.5±0.10ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    395.3±1.11µs     7.5 MB/sec    1.00    396.0±0.51µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.7 MB/sec    1.00      5.4±0.04ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1213.3±3.14µs    13.7 MB/sec    1.00   1214.0±4.47µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.3±0.63µs    20.9 MB/sec    1.00    141.1±1.17µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.02ms    10.1 MB/sec    1.00      4.0±0.05ms    10.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    771.9±7.40µs    21.6 MB/sec    1.00    772.8±8.05µs    21.5 MB/sec
formatter/numpy/globals.py                 1.00     79.4±1.32µs    37.2 MB/sec    1.01     79.9±3.16µs    36.9 MB/sec
formatter/pydantic/types.py                1.00  1613.8±16.69µs    15.8 MB/sec    1.01  1622.3±18.78µs    15.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.11ms     3.2 MB/sec    1.00     12.7±0.06ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.8±5.77µs     8.0 MB/sec    1.01   372.8±17.31µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.12ms     3.9 MB/sec    1.00      6.6±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.04ms     5.9 MB/sec    1.00      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1453.4±11.24µs    11.5 MB/sec    1.00   1441.7±7.73µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.3±1.22µs    19.8 MB/sec    1.01    150.1±1.76µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

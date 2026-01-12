```yaml
number: 4039
title: "Avoid `RUF008` if field annotation is immutable"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/dataclass-immutable-annotation
created_at: 2023-04-20T05:32:22Z
updated_at: 2023-04-23T10:00:40Z
url: https://github.com/astral-sh/ruff/pull/4039
synced_at: 2026-01-12T15:55:14Z
```

# Avoid `RUF008` if field annotation is immutable

---

_@dhruvmanila_

## Summary

Separate out the helper function `is_immutable_annotation` and it's dependencies into the respective crate:
* `is_immutable_annotation` to `ruff_python_semantic`
* Related constants to `ruff_python_stdlib`

I've separated the changes into two commits to ease up the review process.

fixes: #4038 

---

_Comment by @github-actions[bot] on 2023-04-20 05:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.04ms     2.9 MB/sec    1.01     14.3±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.5±1.69µs     6.5 MB/sec    1.00    456.5±0.93µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.01      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1624.3±2.32µs    10.3 MB/sec    1.00   1622.3±2.00µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    180.8±0.37µs    16.3 MB/sec    1.00    177.7±0.46µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.0 MB/sec    1.00      5.8±0.00ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.01   1154.2±0.66µs    14.4 MB/sec    1.00   1143.7±1.91µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    114.9±0.34µs    25.7 MB/sec    1.00    115.1±0.37µs    25.6 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.08ms    10.1 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.09     19.3±0.79ms     2.1 MB/sec    1.00     17.7±0.41ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.8±0.21ms     3.4 MB/sec    1.00      4.6±0.15ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.04   604.8±28.31µs     4.9 MB/sec    1.00   582.2±32.32µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.06      8.2±0.42ms     3.1 MB/sec    1.00      7.7±0.30ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.06      9.8±0.61ms     4.2 MB/sec    1.00      9.3±0.49ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08      2.1±0.16ms     7.8 MB/sec    1.00  1979.3±82.25µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.04   247.3±19.14µs    11.9 MB/sec    1.00   238.4±14.23µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.12      4.5±0.26ms     5.7 MB/sec    1.00      4.0±0.11ms     6.4 MB/sec
parser/large/dataset.py                    1.00      7.6±0.29ms     5.4 MB/sec    1.02      7.7±0.33ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1443.7±76.65µs    11.5 MB/sec    1.02  1474.3±61.48µs    11.3 MB/sec
parser/numpy/globals.py                    1.01    141.8±5.85µs    20.8 MB/sec    1.00    140.9±6.86µs    20.9 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.16ms     7.8 MB/sec    1.01      3.3±0.14ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-20 20:02_

---

_Closed by @charliermarsh on 2023-04-20 20:02_

---

_Comment by @charliermarsh on 2023-04-20 20:02_

Looks great :)

---

_Branch deleted on 2023-04-23 10:00_

---

```yaml
number: 4306
title: "UP011: Fix typo in rule description"
type: pull_request
state: merged
author: aureliojargas
labels:
  - documentation
assignees: []
merged: true
base: main
head: pu011-parentheses
created_at: 2023-05-09T12:39:51Z
updated_at: 2023-05-09T13:23:00Z
url: https://github.com/astral-sh/ruff/pull/4306
synced_at: 2026-01-12T15:55:15Z
```

# UP011: Fix typo in rule description

---

_@aureliojargas_

This rule is about removing the extra parentheses, not parameters :)

Reference:
https://github.com/asottile/pyupgrade#remove-parentheses-from-functoolslru_cache



---

_Merged by @charliermarsh on 2023-05-09 12:49_

---

_Closed by @charliermarsh on 2023-05-09 12:49_

---

_Label `documentation` added by @charliermarsh on 2023-05-09 12:49_

---

_Comment by @charliermarsh on 2023-05-09 12:49_

Good catch, thanks!

---

_Comment by @github-actions[bot] on 2023-05-09 12:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.04ms     2.8 MB/sec    1.01     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    359.8±1.33µs     8.2 MB/sec    1.00    360.6±1.87µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1533.9±5.11µs    10.9 MB/sec    1.00  1538.5±11.56µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±0.30µs    18.0 MB/sec    1.00    164.6±0.99µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.01      3.3±0.04ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.00      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1151.0±2.58µs    14.5 MB/sec    1.00   1152.7±2.84µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    118.0±0.61µs    25.0 MB/sec    1.00    117.8±0.95µs    25.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.18ms     2.4 MB/sec    1.02     17.5±0.37ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.8 MB/sec    1.00      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.3±7.69µs     5.9 MB/sec    1.00    498.6±8.37µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.11ms     3.5 MB/sec    1.01      7.3±0.15ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.06ms     4.7 MB/sec    1.03      8.8±0.14ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.1±19.24µs     9.2 MB/sec    1.02  1835.4±25.83µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.0±6.80µs    14.8 MB/sec    1.02    203.9±6.45µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.04      4.0±0.10ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.9±0.06ms     5.9 MB/sec    1.01      6.9±0.07ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1302.4±14.93µs    12.8 MB/sec    1.01  1319.8±17.64µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    133.7±3.61µs    22.1 MB/sec    1.01    134.7±2.59µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.8 MB/sec    1.01      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-09 13:23_

---

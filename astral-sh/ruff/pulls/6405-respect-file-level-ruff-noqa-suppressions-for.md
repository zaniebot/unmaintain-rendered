```yaml
number: 6405
title: "Respect file-level `# ruff: noqa` suppressions for `unused-noqa` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/noqa
created_at: 2023-08-07T19:55:53Z
updated_at: 2023-08-07T20:33:10Z
url: https://github.com/astral-sh/ruff/pull/6405
synced_at: 2026-01-12T15:55:21Z
```

# Respect file-level `# ruff: noqa` suppressions for `unused-noqa` rule

---

_@charliermarsh_

## Summary

We weren't respecting `# ruff: noqa: RUF100`, i.e., file-level suppressions for the `unused-noqa` rule itself.

Closes https://github.com/astral-sh/ruff/issues/6385.


---

_Comment by @github-actions[bot] on 2023-08-07 20:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.15ms     4.9 MB/sec    1.01      8.4±0.05ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1604.7±18.90µs    10.4 MB/sec    1.01  1623.6±28.56µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    168.3±1.23µs    17.5 MB/sec    1.15  193.7±104.50µs    15.2 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.11ms     7.1 MB/sec    1.00      3.6±0.06ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.6±0.14ms     3.8 MB/sec    1.02     10.8±0.17ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.06ms     5.9 MB/sec    1.00      2.9±0.04ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    316.8±9.14µs     9.3 MB/sec    1.01   319.9±12.27µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.8±0.07ms     5.3 MB/sec    1.01      4.9±0.07ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.09ms     7.5 MB/sec    1.02      5.5±0.08ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1118.0±20.45µs    14.9 MB/sec    1.01  1125.0±17.53µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    115.0±0.30µs    25.7 MB/sec    1.01    116.2±0.32µs    25.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.05ms    10.6 MB/sec    1.01      2.4±0.03ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     13.7±1.32ms     3.0 MB/sec     1.00     13.4±1.37ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.7±0.31ms     6.2 MB/sec     1.00      2.6±0.31ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   273.8±34.69µs    10.8 MB/sec     1.01   277.1±39.09µs    10.6 MB/sec
formatter/pydantic/types.py                1.01      5.5±0.46ms     4.6 MB/sec     1.00      5.5±0.53ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.93ms     2.4 MB/sec     1.02     17.1±0.89ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.54ms     3.6 MB/sec     1.04      4.8±0.58ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.9±63.81µs     5.2 MB/sec     1.03   586.0±73.43µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.39ms     3.4 MB/sec     1.08      8.1±0.76ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.72ms     4.4 MB/sec     1.02      9.5±1.02ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1870.8±202.10µs     8.9 MB/sec    1.03  1934.1±227.34µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   221.2±34.94µs    13.3 MB/sec     1.03   228.8±36.57µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.59ms     5.8 MB/sec     1.00      4.3±0.51ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-07 20:33_

---

_Closed by @charliermarsh on 2023-08-07 20:33_

---

_Branch deleted on 2023-08-07 20:33_

---

_Label `bug` added by @charliermarsh on 2023-08-07 20:33_

---

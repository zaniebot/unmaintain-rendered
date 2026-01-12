```yaml
number: 5318
title: Add dark- and light-mode image modifiers for custom MkDocs themes
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/dark
created_at: 2023-06-22T19:57:00Z
updated_at: 2023-06-22T20:33:25Z
url: https://github.com/astral-sh/ruff/pull/5318
synced_at: 2026-01-12T03:36:54Z
```

# Add dark- and light-mode image modifiers for custom MkDocs themes

---

_Pull request opened by @charliermarsh on 2023-06-22 19:57_

## Summary

Roughly following the docs [here](https://squidfunk.github.io/mkdocs-material/reference/images/#custom-light-scheme).

Closes #5311.

---

_Label `documentation` added by @charliermarsh on 2023-06-22 19:57_

---

_Comment by @github-actions[bot] on 2023-06-22 20:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.02ms     6.2 MB/sec    1.01      6.6±0.03ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1439.6±1.50µs    11.6 MB/sec    1.00   1443.4±1.57µs    11.5 MB/sec
formatter/numpy/globals.py                 1.00    161.9±0.30µs    18.2 MB/sec    1.00    161.8±0.19µs    18.2 MB/sec
formatter/pydantic/types.py                1.01      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.03ms     3.1 MB/sec    1.01     13.2±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.7±0.77µs     7.0 MB/sec    1.02    429.0±0.63µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.0±4.08µs    11.4 MB/sec    1.01   1479.4±2.39µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.9±0.23µs    18.0 MB/sec    1.02    167.7±0.72µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.26ms     3.9 MB/sec    1.00     10.6±0.27ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.09ms     7.4 MB/sec    1.00      2.3±0.09ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   257.4±11.93µs    11.5 MB/sec    1.02   261.9±19.88µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.17ms     4.9 MB/sec    1.02      5.3±0.19ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     20.2±0.37ms     2.0 MB/sec    1.02     20.6±0.54ms  2024.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.17ms     3.1 MB/sec    1.01      5.4±0.16ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   665.5±27.88µs     4.4 MB/sec    1.01   672.2±30.82µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.26ms     2.9 MB/sec    1.02      9.1±0.26ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±0.41ms     3.8 MB/sec    1.02     10.8±0.42ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.05ms     7.5 MB/sec    1.01      2.3±0.06ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   272.3±12.80µs    10.8 MB/sec    1.01   275.1±13.63µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.12ms     5.4 MB/sec    1.03      4.9±0.16ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-22 20:11_

---

_Closed by @charliermarsh on 2023-06-22 20:11_

---

_Branch deleted on 2023-06-22 20:11_

---

```yaml
number: 5479
title: "Add documentation to the `S1XX` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: s1-docs
created_at: 2023-07-03T15:17:18Z
updated_at: 2023-07-10T09:54:53Z
url: https://github.com/astral-sh/ruff/pull/5479
synced_at: 2026-01-12T15:55:18Z
```

# Add documentation to the `S1XX` rules

---

_@tjkuson_

## Summary

Add documentation to the `S1XX` rules (the `flake8-bandit` ['misc tests'](https://bandit.readthedocs.io/en/latest/plugins/index.html#plugin-id-groupings) rule group).

## Test Plan

`python scripts/check_docs_formatted.py && mkdocs serve`


---

_Comment by @github-actions[bot] on 2023-07-03 15:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.02ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.02   1808.6±9.84µs     9.2 MB/sec    1.00   1781.2±6.38µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    196.8±1.49µs    15.0 MB/sec    1.00    196.0±2.56µs    15.1 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.04ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.00     14.0±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.8±0.86µs     8.1 MB/sec    1.00    366.2±0.95µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1474.2±1.73µs    11.3 MB/sec    1.00   1481.2±6.19µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.1±0.20µs    18.5 MB/sec    1.00    159.2±0.30µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.56ms     3.6 MB/sec    1.04     11.6±0.78ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.15ms     6.8 MB/sec    1.02      2.5±0.14ms     6.7 MB/sec
formatter/numpy/globals.py                 1.00   293.4±19.59µs    10.1 MB/sec    1.00   293.3±25.07µs    10.1 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.27ms     4.7 MB/sec    1.01      5.5±0.34ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.07     21.1±1.35ms  1976.4 KB/sec    1.00     19.7±0.73ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.3±0.25ms     3.2 MB/sec    1.00      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   649.5±40.38µs     4.5 MB/sec    1.00   640.6±29.39µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.06      9.1±0.37ms     2.8 MB/sec    1.00      8.6±0.32ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.56ms     4.0 MB/sec    1.00      9.9±0.61ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.14ms     7.8 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   257.5±15.88µs    11.5 MB/sec    1.01   259.1±18.59µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.21ms     5.5 MB/sec    1.00      4.6±0.37ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-05 20:36_

---

_Label `documentation` added by @charliermarsh on 2023-07-05 20:36_

---

_Merged by @charliermarsh on 2023-07-06 17:46_

---

_Closed by @charliermarsh on 2023-07-06 17:46_

---

_Branch deleted on 2023-07-10 09:54_

---

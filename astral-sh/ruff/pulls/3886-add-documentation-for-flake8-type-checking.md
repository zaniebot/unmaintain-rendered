```yaml
number: 3886
title: Add documentation for flake8-type-checking
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: flake8-type-checking-documentation
created_at: 2023-04-05T16:22:24Z
updated_at: 2023-04-05T17:53:38Z
url: https://github.com/astral-sh/ruff/pull/3886
synced_at: 2026-01-12T15:55:14Z
```

# Add documentation for flake8-type-checking

---

_@tjkuson_

Add documentation to the flake8-type-checking rules that check for imports used only for type annotation that are not in a type-checking block.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-04-05 16:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.08ms     2.8 MB/sec    1.02     14.7±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    461.8±1.10µs     6.4 MB/sec    1.00    462.3±1.43µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.5 MB/sec    1.03      7.6±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1651.9±4.57µs    10.1 MB/sec    1.03   1695.9±4.80µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.0±0.40µs    16.2 MB/sec    1.01    184.2±3.79µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.03      3.5±0.01ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.46ms     2.9 MB/sec    1.04     14.5±0.59ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.09ms     4.9 MB/sec    1.01      3.4±0.10ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   360.4±10.34µs     8.2 MB/sec    1.00    360.3±6.31µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.24ms     4.3 MB/sec    1.03      6.1±0.29ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.17ms     5.6 MB/sec    1.03      7.5±0.24ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.6±23.05µs    11.2 MB/sec    1.06  1569.0±46.92µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    158.8±2.02µs    18.6 MB/sec    1.00    156.4±3.41µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.08ms     7.8 MB/sec    1.00      3.3±0.09ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-04-05 17:25_

---

_Merged by @charliermarsh on 2023-04-05 17:30_

---

_Closed by @charliermarsh on 2023-04-05 17:30_

---

_Branch deleted on 2023-04-05 17:53_

---

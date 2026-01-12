```yaml
number: 5079
title: Consider ignore-names in all pep8 naming rules
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/issue_5050
created_at: 2023-06-14T08:28:45Z
updated_at: 2023-06-14T14:57:10Z
url: https://github.com/astral-sh/ruff/pull/5079
synced_at: 2026-01-12T15:55:17Z
```

# Consider ignore-names in all pep8 naming rules

---

_@Thomasdezeeuw_

## Summary

This changes all remaining pep8 naming rules to consider the `ingore-names` argument.

Closes #5050

## Test Plan

Added new tests.

---

_Comment by @github-actions[bot] on 2023-06-14 09:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.02ms     5.5 MB/sec    1.03      7.7±0.03ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1555.1±8.65µs    10.7 MB/sec    1.02   1591.3±7.52µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    149.1±0.54µs    19.8 MB/sec    1.01    150.9±0.65µs    19.6 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.02ms     8.3 MB/sec    1.03      3.1±0.01ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.02     16.7±0.06ms     2.4 MB/sec    1.00     16.4±0.07ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    503.8±2.09µs     5.9 MB/sec    1.00    496.5±1.18µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.03ms     3.6 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.06      8.3±0.03ms     4.9 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05   1785.4±5.16µs     9.3 MB/sec    1.00   1704.1±4.75µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    196.8±1.22µs    15.0 MB/sec    1.00    190.8±1.12µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.7±0.01ms     6.8 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.0±0.06ms     5.1 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1585.8±11.14µs    10.5 MB/sec    1.00   1569.8±9.69µs    10.6 MB/sec
formatter/numpy/globals.py                 1.01    156.0±2.53µs    18.9 MB/sec    1.00    154.8±2.11µs    19.1 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.03ms     8.0 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.24ms     2.5 MB/sec    1.00     16.6±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    434.7±4.91µs     6.8 MB/sec    1.00    432.1±5.51µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.08ms     3.5 MB/sec    1.00      7.2±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.07ms     4.8 MB/sec    1.00      8.4±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1743.2±18.56µs     9.6 MB/sec    1.00  1740.9±17.29µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.2±1.56µs    15.7 MB/sec    1.00    187.7±1.25µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Thomasdezeeuw on 2023-06-14 13:34_

I've added some more ignore paths to the pre-commit CI check based on @konstin recommendation.

---

_Review comment by @konstin on `.pre-commit-config.yaml`:7 on 2023-06-14 13:35_

Sorry, forgot to mention that earlier: This one specifically might fit better in _typos.toml 

---

_@konstin reviewed on 2023-06-14 13:35_

---

_Comment by @Thomasdezeeuw on 2023-06-14 13:46_

:facepalm: now the CI is complaining about a typo in a comment referencing the type, will fix after a round of reviews.

---

_@charliermarsh approved on 2023-06-14 14:00_

Code looks great. Sorry about all the churn with the typo checker...

---

_Merged by @Thomasdezeeuw on 2023-06-14 14:57_

---

_Closed by @Thomasdezeeuw on 2023-06-14 14:57_

---

_Branch deleted on 2023-06-14 14:57_

---

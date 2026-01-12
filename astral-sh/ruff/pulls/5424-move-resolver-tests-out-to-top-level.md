```yaml
number: 5424
title: Move resolver tests out to top-level
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tests
created_at: 2023-06-28T16:51:50Z
updated_at: 2023-06-28T17:25:38Z
url: https://github.com/astral-sh/ruff/pull/5424
synced_at: 2026-01-12T03:36:55Z
```

# Move resolver tests out to top-level

---

_Pull request opened by @charliermarsh on 2023-06-28 16:51_

## Summary

These are really tests for the entire crate.

---

_Label `internal` added by @charliermarsh on 2023-06-28 16:51_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-28 16:51_

---

_Comment by @github-actions[bot] on 2023-06-28 17:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.11      9.3±0.47ms     4.4 MB/sec     1.00      8.4±0.43ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.05  1824.0±104.18µs     9.1 MB/sec    1.00  1742.7±100.36µs     9.6 MB/sec
formatter/numpy/globals.py                 1.14   236.5±15.10µs    12.5 MB/sec     1.00   208.1±11.90µs    14.2 MB/sec
formatter/pydantic/types.py                1.07      4.1±0.29ms     6.2 MB/sec     1.00      3.9±0.21ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.04     16.3±0.85ms     2.5 MB/sec     1.00     15.8±0.70ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.18ms     4.6 MB/sec     1.09      4.0±0.25ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   473.7±26.58µs     6.2 MB/sec     1.16   547.6±26.07µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.39ms     3.7 MB/sec     1.01      7.0±0.36ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.39ms     5.3 MB/sec     1.16      8.9±0.44ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1544.5±54.16µs    10.8 MB/sec     1.09  1681.1±63.91µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±8.61µs    15.5 MB/sec     1.13   214.5±14.51µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.12ms     7.7 MB/sec     1.16      3.9±0.14ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.0±0.49ms     3.4 MB/sec    1.00     11.8±0.39ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.6±0.21ms     6.5 MB/sec    1.00      2.5±0.09ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   287.1±23.70µs    10.3 MB/sec    1.01   288.9±17.94µs    10.2 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.25ms     4.6 MB/sec    1.02      5.7±0.25ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.8±0.49ms     2.1 MB/sec    1.02     20.2±0.75ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.19ms     3.2 MB/sec    1.00      5.2±0.17ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   633.1±24.12µs     4.7 MB/sec    1.00   634.3±23.68µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.29ms     2.9 MB/sec    1.01      8.9±0.27ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.29ms     4.0 MB/sec    1.00     10.1±0.56ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.09ms     7.6 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   252.5±15.86µs    11.7 MB/sec    1.00    248.2±9.84µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.33ms     5.5 MB/sec    1.00      4.5±0.14ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-06-28 17:05_

---

_Merged by @charliermarsh on 2023-06-28 17:25_

---

_Closed by @charliermarsh on 2023-06-28 17:25_

---

_Branch deleted on 2023-06-28 17:25_

---

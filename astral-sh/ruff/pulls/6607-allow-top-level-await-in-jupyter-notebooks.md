```yaml
number: 6607
title: "Allow top-level `await` in Jupyter notebooks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/await
created_at: 2023-08-16T03:41:20Z
updated_at: 2023-08-16T04:14:06Z
url: https://github.com/astral-sh/ruff/pull/6607
synced_at: 2026-01-12T15:55:22Z
```

# Allow top-level `await` in Jupyter notebooks

---

_@charliermarsh_

## Summary

Top-level `await` is allowed in Jupyter notebooks (see: [autoawait](https://ipython.readthedocs.io/en/stable/interactive/autoawait.html)).

Closes https://github.com/astral-sh/ruff/issues/6584.

## Test Plan

Had to test this manually. Created a notebook, verified that the `yield` was flagged but the `await` was not.

<img width="868" alt="Screen Shot 2023-08-15 at 11 40 19 PM" src="https://github.com/astral-sh/ruff/assets/1309177/b2853651-30a6-4dc6-851c-9fe7f694b8e8">


---

_Label `bug` added by @charliermarsh on 2023-08-16 03:41_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-16 03:49_

---

_Comment by @github-actions[bot] on 2023-08-16 03:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.6±0.28ms     8.8 MB/sec     1.02      4.7±0.40ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   931.4±41.45µs    17.9 MB/sec     1.01   941.0±45.45µs    17.7 MB/sec
formatter/numpy/globals.py                 1.00     92.0±4.79µs    32.1 MB/sec     1.01     92.7±6.41µs    31.8 MB/sec
formatter/pydantic/types.py                1.00  1822.9±113.63µs    14.0 MB/sec    1.01  1845.5±101.59µs    13.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.36ms     2.9 MB/sec     1.08     15.4±0.55ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.19ms     4.5 MB/sec     1.05      3.8±0.15ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   517.8±16.02µs     5.7 MB/sec     1.01   520.9±24.41µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.20ms     3.5 MB/sec     1.08      7.9±0.29ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.16ms     5.6 MB/sec     1.07      7.8±0.21ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1591.8±53.96µs    10.5 MB/sec     1.01  1602.9±80.78µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   197.6±14.95µs    14.9 MB/sec     1.00    193.4±7.80µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.09ms     7.9 MB/sec     1.05      3.4±0.13ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.02ms     9.9 MB/sec    1.00      4.1±0.06ms     9.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   781.1±13.61µs    21.3 MB/sec    1.01   785.9±16.21µs    21.2 MB/sec
formatter/numpy/globals.py                 1.00     78.8±1.07µs    37.4 MB/sec    1.01     79.7±1.58µs    37.0 MB/sec
formatter/pydantic/types.py                1.00  1599.5±18.37µs    15.9 MB/sec    1.01  1617.0±42.65µs    15.8 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.10ms     3.2 MB/sec    1.00     12.8±0.19ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.01      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.2±6.70µs     7.9 MB/sec    1.02    381.2±8.47µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.04ms     3.9 MB/sec    1.01      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.04ms     5.8 MB/sec    1.02      7.1±0.05ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1459.9±16.50µs    11.4 MB/sec    1.01  1477.0±28.46µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.9±2.53µs    19.6 MB/sec    1.00    150.6±3.05µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-08-16 03:53_

Looks good, thanks!

For the long term we might want to check if `autoawait` is disabled or not otherwise we might give false negative.

---

_Comment by @charliermarsh on 2023-08-16 03:59_

Makes sense. I think the false positive is more annoying than the false negative is worrisome, so let's proceed.

---

_Merged by @charliermarsh on 2023-08-16 03:59_

---

_Closed by @charliermarsh on 2023-08-16 03:59_

---

_Branch deleted on 2023-08-16 03:59_

---

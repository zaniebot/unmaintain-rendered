```yaml
number: 6321
title: Tweak breaking groups for comprehensions
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/comp-break
created_at: 2023-08-03T23:15:51Z
updated_at: 2023-08-04T14:24:13Z
url: https://github.com/astral-sh/ruff/pull/6321
synced_at: 2026-01-12T02:52:04Z
```

# Tweak breaking groups for comprehensions

---

_Pull request opened by @charliermarsh on 2023-08-03 23:15_

## Summary

Fixes some comprehension formatting by avoiding creating the group for the comprehension itself (so that if it breaks, all parts break on their own lines, e.g. the `for` and the `if` clauses).

Closes https://github.com/astral-sh/ruff/issues/6063.

## Test Plan

Bunch of new fixtures.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 23:15_

---

_Review requested from @konstin by @charliermarsh on 2023-08-03 23:15_

---

_Label `formatter` added by @charliermarsh on 2023-08-03 23:15_

---

_Comment by @github-actions[bot] on 2023-08-03 23:46_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.1±0.51ms     4.5 MB/sec    1.04      9.5±0.37ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1846.3±85.62µs     9.0 MB/sec    1.00  1833.3±70.34µs     9.1 MB/sec
formatter/numpy/globals.py                 1.00   207.2±11.54µs    14.2 MB/sec    1.00   207.5±11.04µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.12ms     6.8 MB/sec    1.05      3.9±0.15ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.04     13.1±0.46ms     3.1 MB/sec    1.00     12.6±0.39ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.14ms     5.1 MB/sec    1.00      3.2±0.08ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   464.5±20.72µs     6.4 MB/sec    1.00   458.6±16.25µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.1±0.26ms     4.2 MB/sec    1.00      5.8±0.18ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.08      6.5±0.17ms     6.3 MB/sec    1.00      6.0±0.15ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1391.0±70.38µs    12.0 MB/sec    1.00  1296.0±32.04µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.04    166.8±7.77µs    17.7 MB/sec    1.00    160.6±6.51µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.05      2.9±0.10ms     8.9 MB/sec    1.00      2.7±0.11ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     12.5±0.46ms     3.3 MB/sec     1.00     12.5±0.68ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.14ms     6.9 MB/sec     1.00      2.4±0.11ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   261.1±18.55µs    11.3 MB/sec     1.05   275.0±21.35µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.26ms     4.9 MB/sec     1.02      5.3±0.26ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.65ms     2.4 MB/sec     1.02     17.4±0.60ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.25ms     3.8 MB/sec     1.02      4.5±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   542.8±33.74µs     5.4 MB/sec     1.00   532.7±29.03µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.34ms     3.3 MB/sec     1.02      7.8±0.29ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.23ms     4.8 MB/sec     1.00      8.6±0.27ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1792.3±125.82µs     9.3 MB/sec    1.00  1781.2±86.13µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   204.3±11.58µs    14.4 MB/sec     1.01   205.4±14.74µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.17ms     6.6 MB/sec     1.00      3.9±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-08-04 06:36_

Please update the PR summary with the similarity index before/after your change to ensure we're improving the formatting overall, and aren't over-fitting on a single usage. 

It may also be good to diff the Black/Ruff differences between the new and old ruff version, e.g. using the django project and review the changes to ensure we aren't regressing anything. 

---

_Comment by @konstin on 2023-08-04 07:47_

stability index (caveat: i picked the lastest run from main instead of the branch base):

name: PR, main
zulip: 0.992 0.992
django: 0.992 0.991
warehouse: 0.978 0.978
transformers: 0.993 0.993
cpython: 0.759 0.759
typeshed: 0.723 0.723

I agree with manually checking django though, i found this helpful in the past.

---

_@konstin approved on 2023-08-04 07:48_

surprised there are no black fixtures for this

---

_Comment by @charliermarsh on 2023-08-04 13:40_

(Reviewed manually, happy with the changes.)

---

_Merged by @charliermarsh on 2023-08-04 14:00_

---

_Closed by @charliermarsh on 2023-08-04 14:00_

---

_Branch deleted on 2023-08-04 14:00_

---

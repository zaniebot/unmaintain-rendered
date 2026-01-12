```yaml
number: 5038
title: "Improve `TypedDict` conversion logic for shadowed builtins and dunder methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up013
created_at: 2023-06-12T21:16:25Z
updated_at: 2023-06-12T21:41:50Z
url: https://github.com/astral-sh/ruff/pull/5038
synced_at: 2026-01-12T03:43:30Z
```

# Improve `TypedDict` conversion logic for shadowed builtins and dunder methods

---

_Pull request opened by @charliermarsh on 2023-06-12 21:16_

## Summary

This PR (1) avoids flagging `TypedDict` and `NamedTuple` conversions when attributes are dunder methods, like `__dict__`, and (2) avoids flagging the `A003` shadowed-attribute rule for `TypedDict` classes at all, where it doesn't really apply (since those attributes are only accessed via subscripting anyway).

Closes #5027.


---

_Merged by @charliermarsh on 2023-06-12 21:23_

---

_Closed by @charliermarsh on 2023-06-12 21:23_

---

_Branch deleted on 2023-06-12 21:23_

---

_Comment by @github-actions[bot] on 2023-06-12 21:41_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      7.0±0.35ms     5.8 MB/sec     1.00      6.9±0.37ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1494.8±104.30µs    11.1 MB/sec    1.00  1484.9±111.55µs    11.2 MB/sec
formatter/numpy/globals.py                 1.00   146.2±14.47µs    20.2 MB/sec     1.00   146.1±12.46µs    20.2 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.24ms     8.8 MB/sec     1.03      3.0±0.20ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.01     15.9±0.89ms     2.6 MB/sec     1.00     15.8±0.87ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.19ms     4.5 MB/sec     1.01      3.7±0.21ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   503.5±29.44µs     5.9 MB/sec     1.00   478.0±31.39µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.0±0.45ms     3.7 MB/sec     1.00      6.6±0.33ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.37ms     5.4 MB/sec     1.00      7.5±0.37ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1648.7±112.04µs    10.1 MB/sec    1.00  1612.8±103.34µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   190.8±14.23µs    15.5 MB/sec     1.00   190.2±12.52µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.15ms     7.6 MB/sec     1.02      3.4±0.19ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.09ms     5.1 MB/sec    1.00      7.8±0.09ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1605.2±24.78µs    10.4 MB/sec    1.00  1601.1±27.81µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    151.2±4.13µs    19.5 MB/sec    1.01    152.7±6.40µs    19.3 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.06ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.03     17.8±0.24ms     2.3 MB/sec    1.00     17.3±0.27ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.07ms     3.8 MB/sec    1.00      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   514.3±13.49µs     5.7 MB/sec    1.00   508.2±11.29µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.12ms     3.4 MB/sec    1.00      7.5±0.19ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.06      9.1±0.13ms     4.5 MB/sec    1.00      8.5±0.13ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1875.8±23.89µs     8.9 MB/sec    1.00  1782.9±39.15µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    204.9±3.84µs    14.4 MB/sec    1.00    199.0±4.51µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.06ms     6.4 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

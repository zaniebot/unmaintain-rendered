```yaml
number: 6300
title: Make formatter ecosystem check failure output better understandable
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: ecosystem_check_failure_output
created_at: 2023-08-03T08:28:55Z
updated_at: 2023-08-03T18:23:26Z
url: https://github.com/astral-sh/ruff/pull/6300
synced_at: 2026-01-12T02:52:04Z
```

# Make formatter ecosystem check failure output better understandable

---

_Pull request opened by @konstin on 2023-08-03 08:28_

**Summary** Prompted by https://github.com/astral-sh/ruff/pull/6257#issuecomment-1661308410, it tried to make the ecosystem script output on failure better understandable. All log messages are now written to a file, which is printed on error. Running locally progress is still shown.

Looking through the log output i saw that we currently log syntax errors in input, which is confusing because they aren't actual errors, but we don't check that these files don't change due to parser regressions or improvements. I added `--files-with-errors` to catch that.

**Test Plan** CI

---

_Label `internal` added by @konstin on 2023-08-03 08:28_

---

_Label `formatter` added by @konstin on 2023-08-03 08:28_

---

_Comment by @github-actions[bot] on 2023-08-03 08:54_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.11ms     4.9 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1636.6±16.06µs    10.2 MB/sec    1.00  1643.5±26.59µs    10.1 MB/sec
formatter/numpy/globals.py                 1.02    185.0±6.66µs    16.0 MB/sec    1.00    181.2±2.05µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.06ms     7.2 MB/sec    1.00      3.5±0.05ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.9±0.04ms     3.7 MB/sec    1.01     11.0±0.05ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.0±2.81µs     7.7 MB/sec    1.00    381.4±0.85µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.04ms     5.2 MB/sec    1.00      4.9±0.04ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.01      5.7±0.03ms     7.1 MB/sec    1.00      5.7±0.02ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1203.3±18.53µs    13.8 MB/sec    1.00   1192.5±4.27µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    131.6±1.75µs    22.4 MB/sec    1.00    130.4±0.30µs    22.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.01ms    10.0 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.58ms     3.8 MB/sec    1.00     10.8±0.47ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.10ms     8.0 MB/sec    1.01      2.1±0.12ms     8.0 MB/sec
formatter/numpy/globals.py                 1.03   246.0±17.39µs    12.0 MB/sec    1.00   239.3±17.07µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.23ms     5.6 MB/sec    1.00      4.6±0.25ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.02     15.1±0.49ms     2.7 MB/sec    1.00     14.9±0.37ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.17ms     4.2 MB/sec    1.00      4.0±0.19ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   502.3±24.40µs     5.9 MB/sec    1.00   498.1±23.90µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.9±0.40ms     3.7 MB/sec    1.00      6.7±0.29ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.34ms     4.9 MB/sec    1.00      8.2±0.31ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1655.6±57.85µs    10.1 MB/sec    1.01  1664.9±77.29µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    202.4±9.82µs    14.6 MB/sec    1.00    198.9±9.25µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.14ms     7.1 MB/sec    1.00      3.6±0.18ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @konstin on 2023-08-03 09:47_

---

_@zanieb approved on 2023-08-03 15:13_

---

_Merged by @konstin on 2023-08-03 18:23_

---

_Closed by @konstin on 2023-08-03 18:23_

---

_Branch deleted on 2023-08-03 18:23_

---

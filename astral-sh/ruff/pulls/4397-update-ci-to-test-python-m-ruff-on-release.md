```yaml
number: 4397
title: "Update CI to test `python -m ruff` on release"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: ci/4394
created_at: 2023-05-12T18:41:53Z
updated_at: 2023-05-12T19:52:48Z
url: https://github.com/astral-sh/ruff/pull/4397
synced_at: 2026-01-12T15:55:15Z
```

# Update CI to test `python -m ruff` on release

---

_@zanieb_

To prevent future regressions like https://github.com/charliermarsh/ruff/issues/4394

---

_Comment by @charliermarsh on 2023-05-12 18:42_

Thank you, great idea.

---

_Comment by @charliermarsh on 2023-05-12 18:42_

Hopefully CI actually fails here? :)

---

_Comment by @zanieb on 2023-05-12 18:44_

@charliermarsh I only updated the `release` workflow. I think we'll need to add a new CI workflow for testing Python package builds if you want coverage per pull request (which is a great idea too).

---

_Comment by @charliermarsh on 2023-05-12 18:46_

Oh right right. Thanks.

---

_Merged by @charliermarsh on 2023-05-12 18:47_

---

_Closed by @charliermarsh on 2023-05-12 18:47_

---

_Comment by @github-actions[bot] on 2023-05-12 18:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.67ms     2.3 MB/sec    1.05     18.3±0.64ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.15ms     3.9 MB/sec    1.00      4.2±0.18ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.3±23.60µs     5.6 MB/sec    1.01   535.6±29.24µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.31ms     3.5 MB/sec    1.02      7.5±0.42ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.22ms     4.8 MB/sec    1.02      8.6±0.25ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1870.3±64.37µs     8.9 MB/sec    1.00  1829.6±58.64µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   214.7±11.42µs    13.7 MB/sec    1.03   221.8±15.51µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.33ms     6.5 MB/sec    1.00      3.9±0.24ms     6.5 MB/sec
parser/large/dataset.py                    1.02      7.0±0.21ms     5.8 MB/sec    1.00      6.9±0.19ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1377.3±56.02µs    12.1 MB/sec    1.00  1375.9±44.07µs    12.1 MB/sec
parser/numpy/globals.py                    1.00    134.7±6.87µs    21.9 MB/sec    1.02   136.8±10.10µs    21.6 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.11ms     8.6 MB/sec    1.04      3.1±0.11ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.49ms     2.4 MB/sec    1.00     16.7±0.44ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.18ms     3.9 MB/sec    1.00      4.3±0.20ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   497.4±23.13µs     5.9 MB/sec    1.02   508.0±21.05µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.19ms     3.6 MB/sec    1.00      7.0±0.21ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.23ms     4.9 MB/sec    1.02      8.5±0.28ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1778.5±99.64µs     9.4 MB/sec    1.00  1765.3±55.27µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.1±4.33µs    15.4 MB/sec    1.04    199.0±7.69µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.9 MB/sec    1.01      3.7±0.12ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.18ms     5.9 MB/sec    1.01      6.9±0.11ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.02  1316.8±47.90µs    12.6 MB/sec    1.00  1296.5±34.73µs    12.8 MB/sec
parser/numpy/globals.py                    1.02    135.2±9.07µs    21.8 MB/sec    1.00    132.0±3.83µs    22.4 MB/sec
parser/pydantic/types.py                   1.03      3.0±0.12ms     8.5 MB/sec    1.00      2.9±0.07ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-05-12 19:52_

thanks!

---

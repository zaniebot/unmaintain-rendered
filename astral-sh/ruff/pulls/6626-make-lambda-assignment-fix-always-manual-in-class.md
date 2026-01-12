```yaml
number: 6626
title: Make lambda-assignment fix always-manual in class bodies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/lambda
created_at: 2023-08-16T17:41:20Z
updated_at: 2023-08-17T01:24:49Z
url: https://github.com/astral-sh/ruff/pull/6626
synced_at: 2026-01-12T15:55:22Z
```

# Make lambda-assignment fix always-manual in class bodies

---

_@charliermarsh_

## Summary

Related to https://github.com/astral-sh/ruff/issues/6620 (although that will only be truly closed once we respect manual fixes on the CLI).



---

_Marked ready for review by @charliermarsh on 2023-08-16 17:41_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-16 17:41_

---

_Label `bug` added by @charliermarsh on 2023-08-16 17:41_

---

_Label `autofix` added by @charliermarsh on 2023-08-16 17:41_

---

_Comment by @github-actions[bot] on 2023-08-16 17:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.6±0.03ms    11.4 MB/sec    1.00      3.5±0.03ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.03    735.7±2.72µs    22.6 MB/sec    1.00    716.7±4.42µs    23.2 MB/sec
formatter/numpy/globals.py                 1.05     76.5±0.29µs    38.6 MB/sec    1.00     72.8±0.32µs    40.5 MB/sec
formatter/pydantic/types.py                1.02   1464.6±4.55µs    17.4 MB/sec    1.00   1429.2±8.11µs    17.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.01ms     4.0 MB/sec    1.01     10.4±0.01ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.02ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    393.9±0.61µs     7.5 MB/sec    1.00    394.8±0.54µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.4±0.01ms     4.7 MB/sec    1.00      5.3±0.00ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.5 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1208.9±1.88µs    13.8 MB/sec    1.01   1220.1±4.80µs    13.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    142.0±3.62µs    20.8 MB/sec    1.01    142.9±0.81µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      2.6±0.09ms     9.9 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.06ms     9.7 MB/sec    1.01      4.2±0.07ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   824.2±11.56µs    20.2 MB/sec    1.01   831.6±33.41µs    20.0 MB/sec
formatter/numpy/globals.py                 1.00     84.1±1.97µs    35.1 MB/sec    1.02     85.5±2.77µs    34.5 MB/sec
formatter/pydantic/types.py                1.00  1679.1±36.54µs    15.2 MB/sec    1.02  1704.8±44.48µs    15.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.20ms     3.2 MB/sec    1.01     13.1±0.20ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.6±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    444.6±6.49µs     6.6 MB/sec    1.00    441.5±6.51µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.16ms     3.8 MB/sec    1.00      6.8±0.13ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.1±0.14ms     5.7 MB/sec    1.00      7.0±0.13ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1508.3±19.84µs    11.0 MB/sec    1.00  1501.5±23.54µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.0±3.97µs    16.6 MB/sec    1.00    178.2±7.13µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.07ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-17 00:03_

---

_Merged by @charliermarsh on 2023-08-17 01:24_

---

_Closed by @charliermarsh on 2023-08-17 01:24_

---

_Branch deleted on 2023-08-17 01:24_

---

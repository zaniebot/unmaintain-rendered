```yaml
number: 4714
title: Add ability to generate snapshot tests on code snippets 
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/test-snippet
created_at: 2023-05-29T21:57:42Z
updated_at: 2023-05-29T22:55:51Z
url: https://github.com/astral-sh/ruff/pull/4714
synced_at: 2026-01-12T15:55:16Z
```

# Add ability to generate snapshot tests on code snippets 

---

_@charliermarsh_

## Summary

This PR formalizes the ability to test inline code snippets with our snapshot testing infrastructure. We already used this pattern in `pandas_vet`, but it was one-off, and didn't benefit from all the infra we'd put in place to support snapshot testing of committed Python files.

## Test Plan

`cargo test`


---

_Renamed from "Add ability to test snippets" to "Add ability to generate snapshot tests on code snippets " by @charliermarsh on 2023-05-29 21:57_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-29 21:57_

---

_Label `internal` added by @charliermarsh on 2023-05-29 21:57_

---

_Merged by @charliermarsh on 2023-05-29 22:36_

---

_Closed by @charliermarsh on 2023-05-29 22:36_

---

_Branch deleted on 2023-05-29 22:36_

---

_Comment by @github-actions[bot] on 2023-05-29 22:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.00     14.1±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.2±1.21µs     6.9 MB/sec    1.00    426.6±0.72µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1514.7±2.91µs    11.0 MB/sec    1.00   1511.4±3.50µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.9±0.34µs    17.2 MB/sec    1.00    171.6±0.59µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.1±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1024.9±3.85µs    16.2 MB/sec    1.00   1018.7±0.91µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.7±0.41µs    27.9 MB/sec    1.00    105.6±0.29µs    28.0 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.15ms     2.4 MB/sec    1.00     16.8±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.03ms     3.8 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    442.3±5.40µs     6.7 MB/sec    1.02    450.0±5.04µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.6 MB/sec    1.00      7.2±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.04ms     4.8 MB/sec    1.01      8.5±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1769.8±10.57µs     9.4 MB/sec    1.02  1803.6±16.99µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.7±1.26µs    15.2 MB/sec    1.01    197.1±4.95µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.01      3.9±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.02ms     6.1 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1262.9±11.95µs    13.2 MB/sec    1.00   1268.3±9.90µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.3±0.95µs    22.5 MB/sec    1.00    131.0±0.82µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.0 MB/sec    1.01      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

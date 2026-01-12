```yaml
number: 4696
title: Remove impossible states from version-comparison representation
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/block-metadata
created_at: 2023-05-28T21:59:50Z
updated_at: 2023-05-28T22:28:40Z
url: https://github.com/astral-sh/ruff/pull/4696
synced_at: 2026-01-12T03:50:03Z
```

# Remove impossible states from version-comparison representation

---

_Pull request opened by @charliermarsh on 2023-05-28 21:59_

## Summary

I found some of the existing codepaths (added long ago) hard to follow in a recent review, so doing a non-behavioral refactor to make the cases clearer and more explicit.

---

_Merged by @charliermarsh on 2023-05-28 22:08_

---

_Closed by @charliermarsh on 2023-05-28 22:08_

---

_Branch deleted on 2023-05-28 22:08_

---

_Comment by @github-actions[bot] on 2023-05-28 22:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.10ms     2.8 MB/sec    1.02     15.0±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.0±3.94µs     6.9 MB/sec    1.01    428.7±0.80µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.02      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.05      7.6±0.03ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1571.1±4.37µs    10.6 MB/sec    1.03   1619.4±3.55µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.3±1.15µs    17.0 MB/sec    1.03    178.0±2.47µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.8 MB/sec    1.03      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.01      5.2±0.00ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1025.1±0.54µs    16.2 MB/sec    1.00   1029.7±0.39µs    16.2 MB/sec
parser/numpy/globals.py                    1.00    106.2±0.13µs    27.8 MB/sec    1.00    106.3±0.87µs    27.7 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.01      2.2±0.01ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.17ms     2.4 MB/sec    1.02     17.1±0.83ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.1±6.71µs     5.9 MB/sec    1.02   507.8±12.78µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.01      7.1±0.15ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.11ms     4.9 MB/sec    1.00      8.3±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.3±29.97µs     9.4 MB/sec    1.00  1779.4±35.15µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.0±4.84µs    14.4 MB/sec    1.02    208.3±9.74µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.00      3.8±0.15ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.5±0.09ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1235.0±22.31µs    13.5 MB/sec    1.00  1232.5±16.02µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    125.6±2.41µs    23.5 MB/sec    1.00    126.2±2.76µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.01      2.8±0.04ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

```yaml
number: 3706
title: Fix Ruff pre-commit hook errors
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-ruff-pre-commit-hook-errors
created_at: 2023-03-24T02:00:41Z
updated_at: 2023-03-24T03:16:08Z
url: https://github.com/astral-sh/ruff/pull/3706
synced_at: 2026-01-12T04:39:45Z
```

# Fix Ruff pre-commit hook errors

---

_Pull request opened by @JonathanPlasse on 2023-03-24 02:00_

use `--extend-ignore=E203` as it should not be present. 

---

_Comment by @github-actions[bot] on 2023-03-24 02:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.52ms     2.6 MB/sec    1.06     16.3±0.40ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.12ms     4.2 MB/sec    1.04      4.1±0.12ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   555.5±18.64µs     5.3 MB/sec    1.01   560.2±17.30µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.21ms     3.8 MB/sec    1.06      7.1±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.17ms     4.8 MB/sec    1.06      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1824.9±57.44µs     9.1 MB/sec    1.11      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.4±7.65µs    14.7 MB/sec    1.11    224.3±5.37µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.13ms     6.6 MB/sec    1.13      4.4±0.04ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.40ms     2.7 MB/sec    1.01     15.4±0.19ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    533.2±7.07µs     5.5 MB/sec    1.00    535.6±8.27µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.07ms     3.8 MB/sec    1.01      6.8±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     4.9 MB/sec    1.01      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1821.7±21.36µs     9.1 MB/sec    1.00  1823.4±20.84µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.9±3.14µs    14.8 MB/sec    1.01    200.7±5.06µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-24 02:52_

---

_Closed by @charliermarsh on 2023-03-24 02:52_

---

_Branch deleted on 2023-03-24 03:16_

---

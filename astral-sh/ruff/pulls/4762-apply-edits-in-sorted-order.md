```yaml
number: 4762
title: Apply edits in sorted order
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sort-edits
created_at: 2023-05-31T17:18:51Z
updated_at: 2023-05-31T17:46:54Z
url: https://github.com/astral-sh/ruff/pull/4762
synced_at: 2026-01-12T15:55:16Z
```

# Apply edits in sorted order

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We allow users to store edits in any order, but there's an assumption (in the fix application code) that they're stored in _sorted_ order. This PR modifies the fixer to sort the edits, thereby removing the responsibility from the user.

This is a revival of #4210, in response to a need that arose in https://github.com/charliermarsh/ruff/pull/4742#discussion_r1211233416.


---

_Merged by @charliermarsh on 2023-05-31 17:26_

---

_Closed by @charliermarsh on 2023-05-31 17:26_

---

_Branch deleted on 2023-05-31 17:26_

---

_Comment by @github-actions[bot] on 2023-05-31 17:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.9±1.02ms  1988.6 KB/sec    1.00     20.8±0.77ms  2002.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.21ms     3.4 MB/sec    1.00      4.9±0.26ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   611.4±34.26µs     4.8 MB/sec    1.00   604.9±33.79µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.42ms     3.0 MB/sec    1.01      8.5±0.36ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.46ms     4.1 MB/sec    1.00      9.8±0.34ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.11ms     7.8 MB/sec    1.00      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.01   252.5±13.33µs    11.7 MB/sec    1.00   251.0±20.82µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.21ms     5.7 MB/sec    1.00      4.5±0.19ms     5.7 MB/sec
parser/large/dataset.py                    1.01      7.8±0.27ms     5.2 MB/sec    1.00      7.7±0.29ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.03  1519.4±59.38µs    11.0 MB/sec    1.00  1470.6±71.44µs    11.3 MB/sec
parser/numpy/globals.py                    1.02   154.2±11.03µs    19.1 MB/sec    1.00    151.6±9.67µs    19.5 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.17ms     7.7 MB/sec    1.00      3.3±0.14ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.10ms     2.5 MB/sec    1.00     16.4±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.9±6.67µs     5.9 MB/sec    1.00    499.5±5.56µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      7.0±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1748.9±15.11µs     9.5 MB/sec    1.00  1754.5±15.08µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.9±3.77µs    14.5 MB/sec    1.02    207.2±9.64µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.02      3.8±0.19ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1205.0±11.68µs    13.8 MB/sec    1.00  1203.5±14.19µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    124.0±1.49µs    23.8 MB/sec    1.00    124.3±2.20µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.4 MB/sec    1.00      2.7±0.02ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

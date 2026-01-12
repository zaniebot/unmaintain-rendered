```yaml
number: 4698
title: "markdownlint: enforce 100 char max length"
type: pull_request
state: merged
author: jlaneve
labels: []
assignees: []
merged: true
base: main
head: markdown-formatting
created_at: 2023-05-28T23:52:09Z
updated_at: 2023-05-29T02:46:02Z
url: https://github.com/astral-sh/ruff/pull/4698
synced_at: 2026-01-12T15:55:16Z
```

# markdownlint: enforce 100 char max length

---

_@jlaneve_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR enforces line length consistency across markdown files. Should hopefully save @charliermarsh from jumping in and having to format himself 🙂 

## Test Plan

<!-- How was it tested? -->

n/a


---

_Marked ready for review by @jlaneve on 2023-05-28 23:54_

---

_Comment by @github-actions[bot] on 2023-05-29 00:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.11ms     2.4 MB/sec    1.00     16.7±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.3±3.79µs     5.9 MB/sec    1.00    501.6±6.77µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.7 MB/sec    1.00      7.0±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.00      8.1±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1778.0±9.99µs     9.4 MB/sec    1.00  1784.0±11.38µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.2±1.44µs    14.6 MB/sec    1.00    202.3±1.64µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.02ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.1±0.03ms     6.6 MB/sec    1.00      6.1±0.03ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1218.9±7.71µs    13.7 MB/sec    1.00   1214.2±8.44µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    125.1±1.04µs    23.6 MB/sec    1.01    126.3±0.91µs    23.4 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.02ms     9.6 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.21ms     2.4 MB/sec    1.00     17.2±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.3±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.7±8.77µs     5.8 MB/sec    1.01   516.5±11.83µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.09ms     3.5 MB/sec    1.01      7.3±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.07ms     4.8 MB/sec    1.01      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1806.7±22.73µs     9.2 MB/sec    1.01  1822.4±28.19µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.1±4.20µs    14.2 MB/sec    1.01    208.8±6.53µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.01      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.07ms     6.2 MB/sec    1.01      6.6±0.09ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1240.9±12.81µs    13.4 MB/sec    1.01  1255.2±20.80µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    126.3±2.22µs    23.4 MB/sec    1.01    127.2±2.87µs    23.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.11ms     9.1 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-29 02:45_

---

_Closed by @charliermarsh on 2023-05-29 02:45_

---

_Comment by @charliermarsh on 2023-05-29 02:46_

Thank you! Looks great.

---

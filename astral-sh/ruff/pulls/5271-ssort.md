```yaml
number: 5271
title: Ssort
type: pull_request
state: closed
author: jgberry
labels: []
assignees: []
draft: true
base: main
head: ssort
created_at: 2023-06-21T22:06:38Z
updated_at: 2023-10-19T15:12:00Z
url: https://github.com/astral-sh/ruff/pull/5271
synced_at: 2026-01-12T02:32:41Z
```

# Ssort

---

_Pull request opened by @jgberry on 2023-06-21 22:06_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #3946.

Draft PR for tracking work being done on ssort. Work in progress.

---

_Comment by @github-actions[bot] on 2023-06-21 22:47_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.3±0.01ms     4.9 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.02   1770.9±3.18µs     9.4 MB/sec    1.00   1734.9±2.12µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    188.7±0.29µs    15.6 MB/sec    1.00    188.3±0.62µs    15.7 MB/sec
formatter/pydantic/types.py                1.03      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.02ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.03ms     2.9 MB/sec    1.01     14.0±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.02      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    368.0±0.74µs     8.0 MB/sec    1.00    363.3±1.76µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.07ms     5.7 MB/sec    1.02      7.3±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1481.2±3.60µs    11.2 MB/sec    1.01   1501.2±2.49µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.9±1.59µs    18.5 MB/sec    1.00    160.3±0.20µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.01      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.7±0.12ms     4.2 MB/sec    1.00      9.4±0.22ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.07ms     8.0 MB/sec    1.00      2.0±0.03ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    227.1±4.21µs    13.0 MB/sec    1.01    228.4±8.02µs    12.9 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.07ms     5.6 MB/sec    1.00      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.26ms     2.5 MB/sec    1.00     16.1±0.32ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.00      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    514.3±8.11µs     5.7 MB/sec    1.01    517.4±9.72µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec    1.04      7.4±0.12ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.10ms     4.9 MB/sec    1.00      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1774.2±26.21µs     9.4 MB/sec    1.00  1749.3±27.53µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    207.9±3.95µs    14.2 MB/sec    1.00    202.1±5.72µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.05ms     6.8 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-22 00:27_

Very cool, feel free to ask questions if we can be helpful.

---

_Comment by @jgberry on 2023-06-22 00:38_

Will do! I plan on porting over the primary logic first and then doing the ruff integration second, so might be a little while before I get there. But, I'm sure they'll be plenty of questions when I do!

---

_Comment by @zanieb on 2023-10-19 15:11_

I'm going to close this for book keeping since it is not ready to be reviewed and hasn't been worked on for a few months. Feel free to re-open in the future though!

---

_Closed by @zanieb on 2023-10-19 15:12_

---

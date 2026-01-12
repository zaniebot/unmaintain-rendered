```yaml
number: 5808
title: "Format `AsyncFor`"
type: pull_request
state: merged
author: lkh42t
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-async-for
created_at: 2023-07-16T14:44:37Z
updated_at: 2023-07-17T14:51:20Z
url: https://github.com/astral-sh/ruff/pull/5808
synced_at: 2026-01-12T03:30:21Z
```

# Format `AsyncFor`

---

_Pull request opened by @lkh42t on 2023-07-16 14:44_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `FormatStmtAsyncFor` by sharing impl with `FormatStmtFor` like `With` / `AsyncWith`.

## Test Plan

Existing snapshots.

---

_Comment by @github-actions[bot] on 2023-07-16 15:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1872.0±2.53µs     8.9 MB/sec    1.00  1866.2±16.48µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    210.4±0.26µs    14.0 MB/sec    1.00    209.1±0.17µs    14.1 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.04ms     3.1 MB/sec    1.01     13.3±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.01      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.1±0.61µs     6.5 MB/sec    1.00    452.7±0.61µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.01      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.0±4.32µs    11.3 MB/sec    1.01   1484.8±3.40µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.7±0.50µs    17.5 MB/sec    1.01    169.7±0.22µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.24ms     3.7 MB/sec    1.01     11.1±0.14ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    247.5±7.38µs    11.9 MB/sec    1.00    247.8±7.51µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.01      4.8±0.08ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.16ms     2.6 MB/sec    1.00     15.7±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.01      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.5±8.80µs     5.9 MB/sec    1.01    509.5±9.96µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.02      7.2±0.14ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.01      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1725.4±14.83µs     9.7 MB/sec    1.01  1739.8±21.64µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.1±3.27µs    14.5 MB/sec    1.01    204.7±6.16µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-16 18:17_

---

_Label `formatter` added by @konstin on 2023-07-16 18:18_

---

_@MichaReiser approved on 2023-07-17 08:38_

---

_Comment by @MichaReiser on 2023-07-17 08:38_

Thank you!

---

_Merged by @MichaReiser on 2023-07-17 08:38_

---

_Closed by @MichaReiser on 2023-07-17 08:38_

---

_Branch deleted on 2023-07-17 14:51_

---

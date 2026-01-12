```yaml
number: 6361
title: Respect typing_extensions imports of Annotated for B006.
type: pull_request
state: merged
author: PIG208
labels:
  - bug
assignees: []
merged: true
base: main
head: immutable
created_at: 2023-08-05T06:33:05Z
updated_at: 2023-08-05T18:01:17Z
url: https://github.com/astral-sh/ruff/pull/6361
synced_at: 2026-01-12T15:55:21Z
```

# Respect typing_extensions imports of Annotated for B006.

---

_@PIG208_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
`typing_extensions.Annotated` should be treated the same way as `typing.Annotated`.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-05 06:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.35ms     4.3 MB/sec    1.01      9.5±0.39ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1874.4±43.78µs     8.9 MB/sec    1.00  1882.5±66.73µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    213.3±9.62µs    13.8 MB/sec    1.08   230.3±19.52µs    12.8 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.13ms     6.5 MB/sec    1.00      3.9±0.14ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.30ms     3.4 MB/sec    1.10     13.3±0.43ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.09ms     5.1 MB/sec    1.02      3.3±0.12ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   473.4±15.42µs     6.2 MB/sec    1.00   468.6±18.93µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.17ms     4.5 MB/sec    1.04      5.9±0.19ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.2±0.20ms     6.5 MB/sec    1.03      6.5±0.18ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1369.3±51.17µs    12.2 MB/sec    1.01  1377.9±25.34µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    170.0±7.05µs    17.4 MB/sec    1.00    167.2±7.56µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.8±0.09ms     9.0 MB/sec    1.00      2.8±0.07ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.11ms     4.1 MB/sec    1.02     10.0±0.12ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1898.1±21.18µs     8.8 MB/sec    1.02  1942.4±23.71µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    211.9±4.69µs    13.9 MB/sec    1.02    215.3±6.92µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.03      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.20ms     3.2 MB/sec    1.05     13.5±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.01      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.0±6.35µs     6.9 MB/sec    1.00    426.2±4.88µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.07ms     4.4 MB/sec    1.05      6.1±0.11ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.06ms     6.0 MB/sec    1.00      6.7±0.06ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1408.6±17.32µs    11.8 MB/sec    1.00  1403.9±32.00µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.8±2.59µs    18.5 MB/sec    1.01    160.6±4.13µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.5 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-08-05 14:58_

Hey @PIG208, thanks for contributing!

Could you add a test case for this?

---

_Comment by @PIG208 on 2023-08-05 17:28_

Sure! Updated with a snapshot.

---

_@charliermarsh approved on 2023-08-05 17:30_

---

_Label `bug` added by @charliermarsh on 2023-08-05 17:31_

---

_Comment by @charliermarsh on 2023-08-05 17:31_

Thx!

---

_Merged by @charliermarsh on 2023-08-05 17:39_

---

_Closed by @charliermarsh on 2023-08-05 17:39_

---

_Branch deleted on 2023-08-05 17:50_

---

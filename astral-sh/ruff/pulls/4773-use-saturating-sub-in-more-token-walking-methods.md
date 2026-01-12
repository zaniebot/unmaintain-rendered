```yaml
number: 4773
title: Use saturating_sub in more token-walking methods
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/saturating
created_at: 2023-06-01T02:28:57Z
updated_at: 2023-06-01T21:16:33Z
url: https://github.com/astral-sh/ruff/pull/4773
synced_at: 2026-01-12T03:50:03Z
```

# Use saturating_sub in more token-walking methods

---

_Pull request opened by @charliermarsh on 2023-06-01 02:28_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I got this feedback in a separate PR from @MichaReiser, on some new code, and am wondering if we want to do this everywhere?


---

_Comment by @github-actions[bot] on 2023-06-01 02:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.05ms     2.9 MB/sec    1.00     13.9±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.7±0.38µs     7.0 MB/sec    1.00    421.2±2.15µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1495.2±5.00µs    11.1 MB/sec    1.00   1495.4±4.41µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.2±0.26µs    17.3 MB/sec    1.00    170.4±0.24µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.00ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1019.5±3.33µs    16.3 MB/sec    1.00   1012.5±1.05µs    16.4 MB/sec
parser/numpy/globals.py                    1.01    105.7±0.37µs    27.9 MB/sec    1.00    105.0±0.26µs    28.1 MB/sec
parser/pydantic/types.py                   1.03      2.3±0.04ms    11.1 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     12.6±0.27ms     3.2 MB/sec    1.00     12.3±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.03ms     5.2 MB/sec    1.00      3.2±0.03ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   329.1±15.75µs     9.0 MB/sec    1.00   330.4±14.82µs     8.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.06ms     4.8 MB/sec    1.00      5.3±0.07ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.1±0.05ms     6.6 MB/sec    1.01      6.2±0.06ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1296.8±19.15µs    12.8 MB/sec    1.00  1299.1±14.74µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    141.4±3.09µs    20.9 MB/sec    1.00    140.0±1.92µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
parser/large/dataset.py                    1.00      4.9±0.04ms     8.4 MB/sec    1.00      4.9±0.05ms     8.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   916.8±11.90µs    18.2 MB/sec    1.00   916.7±12.25µs    18.2 MB/sec
parser/numpy/globals.py                    1.00     95.4±1.38µs    30.9 MB/sec    1.01     96.0±1.82µs    30.7 MB/sec
parser/pydantic/types.py                   1.00      2.1±0.03ms    12.4 MB/sec    1.00      2.1±0.03ms    12.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-01 02:38_

---

_@MichaReiser approved on 2023-06-01 05:43_

---

_Merged by @charliermarsh on 2023-06-01 21:16_

---

_Closed by @charliermarsh on 2023-06-01 21:16_

---

_Branch deleted on 2023-06-01 21:16_

---

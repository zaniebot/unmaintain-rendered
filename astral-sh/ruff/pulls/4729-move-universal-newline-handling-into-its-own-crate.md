```yaml
number: 4729
title: Move universal newline handling into its own crate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/newlines
created_at: 2023-05-30T16:17:17Z
updated_at: 2023-05-31T16:00:49Z
url: https://github.com/astral-sh/ruff/pull/4729
synced_at: 2026-01-12T03:50:03Z
```

# Move universal newline handling into its own crate

---

_Pull request opened by @charliermarsh on 2023-05-30 16:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I want to use this is a new crate that shouldn't depend on the AST. In general, newline handling is generic enough that it shouldn't be coupled to the AST utilities.



---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-30 16:17_

---

_Comment by @github-actions[bot] on 2023-05-30 16:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.05ms     2.9 MB/sec    1.01     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.7±0.62µs     7.0 MB/sec    1.01    422.9±0.54µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.8±2.17µs    11.2 MB/sec    1.00   1492.1±4.21µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±0.39µs    17.5 MB/sec    1.01    170.0±2.07µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.02ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.2±0.02ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1018.4±0.41µs    16.4 MB/sec    1.00   1019.5±0.68µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.3±0.63µs    28.0 MB/sec    1.00    105.1±0.31µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.20ms     2.4 MB/sec    1.00     17.1±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.00      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   449.7±20.37µs     6.6 MB/sec    1.00    447.5±4.65µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.00      7.3±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.06ms     4.7 MB/sec    1.00      8.5±0.07ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1801.0±15.80µs     9.2 MB/sec    1.01  1812.0±19.84µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.5±1.79µs    14.9 MB/sec    1.02    200.8±6.16µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.03ms     6.5 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1270.2±10.38µs    13.1 MB/sec    1.00   1265.0±7.28µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.8±0.88µs    22.6 MB/sec    1.00    130.7±0.90µs    22.6 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.02ms     8.9 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-31 07:27_

This is a tiny crate. I wonder if we could come up with a more-generic name that would potentially allow us to move more into it (`ruff_whitespace`, `ruff_trivia`) but maybe it's best to leave it as is because whitespace handling is language specific (some languages support unicode whitespace characters)

---

_Merged by @charliermarsh on 2023-05-31 16:00_

---

_Closed by @charliermarsh on 2023-05-31 16:00_

---

_Branch deleted on 2023-05-31 16:00_

---

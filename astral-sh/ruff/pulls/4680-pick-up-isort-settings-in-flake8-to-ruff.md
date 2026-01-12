```yaml
number: 4680
title: Pick up isort settings in flake8-to-ruff
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: pick-up-isort-settings-in-flake8-to-ruff
created_at: 2023-05-27T15:11:37Z
updated_at: 2023-10-17T11:07:13Z
url: https://github.com/astral-sh/ruff/pull/4680
synced_at: 2026-01-12T02:32:41Z
```

# Pick up isort settings in flake8-to-ruff

---

_Pull request opened by @JonathanPlasse on 2023-05-27 15:11_

- Close #1749
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-05-27 15:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.08ms     2.9 MB/sec    1.00     14.2±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.3±1.12µs     7.0 MB/sec    1.02    428.8±0.68µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.05ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1495.1±2.33µs    11.1 MB/sec    1.00   1498.1±4.17µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.4±0.46µs    17.5 MB/sec    1.02    171.4±1.28µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.3±0.01ms     7.7 MB/sec    1.01      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1039.1±0.56µs    16.0 MB/sec    1.02  1058.0±11.13µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    107.6±0.22µs    27.4 MB/sec    1.00    108.0±0.51µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.3 MB/sec    1.01      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.16ms     2.5 MB/sec    1.00     16.5±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.08ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   504.7±11.07µs     5.8 MB/sec    1.00    498.3±8.25µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.08ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1762.7±16.91µs     9.4 MB/sec    1.00  1754.1±31.24µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.8±3.82µs    14.4 MB/sec    1.01    206.3±7.15µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.11ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.3 MB/sec    1.01      6.5±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1222.4±18.06µs    13.6 MB/sec    1.01  1229.5±22.35µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    124.6±1.81µs    23.7 MB/sec    1.01    125.5±2.01µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.3 MB/sec    1.01      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-12 18:44_

Closing for now to keep the PR queue actionable where we can -- can always re-open if this gets picked back up!

---

_Closed by @charliermarsh on 2023-06-12 18:44_

---

_Branch deleted on 2023-10-17 11:07_

---

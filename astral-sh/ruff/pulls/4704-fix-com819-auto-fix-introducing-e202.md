```yaml
number: 4704
title: Fix COM819 auto-fix introducing E202
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: fix-com819-auto-fix-introducing-e202
created_at: 2023-05-29T12:21:13Z
updated_at: 2023-10-17T11:07:10Z
url: https://github.com/astral-sh/ruff/pull/4704
synced_at: 2026-01-12T15:55:16Z
```

# Fix COM819 auto-fix introducing E202

---

_@JonathanPlasse_

- Close #1975
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

COM819 now removes the spaces after the prohibited comma to avoid introducing `E202`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

The fixture already contained examples with spaces after the prohibited comma.
So, the snapshot has been updated.

<!-- How was it tested? -->


---

_@JonathanPlasse reviewed on 2023-05-29 12:22_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_commas/rules/trailing_commas.rs`:335 on 2023-05-29 12:22_

No token other than spaces should be between the comma and the closing bracket.

---

_Comment by @github-actions[bot] on 2023-05-29 12:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.05ms     2.7 MB/sec    1.01     15.0±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.1±0.82µs     7.9 MB/sec    1.00    373.5±1.63µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.02      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1578.3±2.17µs    10.6 MB/sec    1.01   1595.0±3.97µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.1±0.90µs    17.0 MB/sec    1.01    174.5±1.28µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.0 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1137.5±1.56µs    14.6 MB/sec    1.00   1122.7±1.04µs    14.8 MB/sec
parser/numpy/globals.py                    1.02    116.2±0.54µs    25.4 MB/sec    1.00    114.3±0.33µs    25.8 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.09ms     2.4 MB/sec    1.00     16.8±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    448.1±8.72µs     6.6 MB/sec    1.00    443.3±5.69µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.5 MB/sec    1.00      7.2±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.03ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.2±13.18µs     9.4 MB/sec    1.00  1774.9±11.19µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.5±1.42µs    15.2 MB/sec    1.01    195.4±6.89µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.10      7.3±0.02ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1262.0±7.79µs    13.2 MB/sec    1.08   1368.4±5.56µs    12.2 MB/sec
parser/numpy/globals.py                    1.00    130.5±1.40µs    22.6 MB/sec    1.07    139.0±0.74µs    21.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.0 MB/sec    1.08      3.1±0.01ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-29 20:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_commas/snapshots/ruff__rules__flake8_commas__tests__COM81.py.snap`:551 on 2023-05-29 20:49_

Hmm, this strikes me as an incorrect fix though. We should probably (?) be mirroring the spacing around the opening bracket, though I'm not sure it's even worth accounting for this in this rule.

---

_@charliermarsh requested changes on 2023-05-29 20:50_

I'm not sure on this one -- my current thinking is that I'd rather leave this to formatters, or to `E202` as an autofix, rather than make this rule whitespace-aware given all the cases it may need to accomodate.

---

_@JonathanPlasse reviewed on 2023-05-29 20:54_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_commas/snapshots/ruff__rules__flake8_commas__tests__COM81.py.snap`:551 on 2023-05-29 20:54_

According to [PEP8 - Pet Peeves](https://peps.python.org/pep-0008/#pet-peeves), this style does not respect PEP8.

---

_Comment by @JonathanPlasse on 2023-05-29 20:55_

I am ok with closing this PR and leaving the auto-fix to `E202`.

---

_@charliermarsh reviewed on 2023-05-29 20:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_commas/snapshots/ruff__rules__flake8_commas__tests__COM81.py.snap`:551 on 2023-05-29 20:57_

I agree, I'm just not sure that this rule should be modifying it.

---

_Closed by @JonathanPlasse on 2023-05-29 21:02_

---

_Branch deleted on 2023-10-17 11:07_

---

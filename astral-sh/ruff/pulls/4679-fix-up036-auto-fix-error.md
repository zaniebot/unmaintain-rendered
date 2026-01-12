```yaml
number: 4679
title: Fix UP036 auto-fix error
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-up036-auto-fix-error
created_at: 2023-05-27T13:04:11Z
updated_at: 2023-05-28T05:22:25Z
url: https://github.com/astral-sh/ruff/pull/4679
synced_at: 2026-01-12T15:55:16Z
```

# Fix UP036 auto-fix error

---

_@JonathanPlasse_

- Close #4522
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `BlockMetadata` was incorrect. It was picking up `else` and `elif` tokens inside the `if` body.
Now, it will skip all tokens inside the `if` body.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

A fixture with the failing auto-fix has been added.
<!-- How was it tested? -->


---

_@JonathanPlasse reviewed on 2023-05-27 13:06_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/outdated_version_block.rs`:63 on 2023-05-27 13:06_

Using `located.start()` does not seem to change the behavior.

---

_Comment by @github-actions[bot] on 2023-05-27 13:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.32ms     2.8 MB/sec    1.00     14.5±0.38ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.8±1.29µs     7.0 MB/sec    1.00    425.0±2.26µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.09ms     4.3 MB/sec    1.00      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.07ms     6.0 MB/sec    1.01      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1497.8±4.09µs    11.1 MB/sec    1.00   1499.0±1.45µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±0.45µs    17.3 MB/sec    1.00    170.9±0.31µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1019.4±1.28µs    16.3 MB/sec    1.00   1022.1±1.19µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.4±0.18µs    28.0 MB/sec    1.00    105.9±0.37µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.43ms     2.4 MB/sec    1.00     16.7±0.41ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.14ms     3.9 MB/sec    1.00      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   507.0±17.74µs     5.8 MB/sec    1.00    500.0±6.72µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.17ms     3.6 MB/sec    1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.08      8.8±0.42ms     4.6 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1804.9±43.81µs     9.2 MB/sec    1.00  1761.1±18.05µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.0±6.50µs    14.2 MB/sec    1.00   207.9±11.09µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.08ms     6.7 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.14      7.5±0.12ms     5.4 MB/sec    1.00      6.6±0.12ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.12  1388.1±25.30µs    12.0 MB/sec    1.00  1241.8±22.32µs    13.4 MB/sec
parser/numpy/globals.py                    1.09    137.1±3.08µs    21.5 MB/sec    1.00    125.4±2.57µs    23.5 MB/sec
parser/pydantic/types.py                   1.11      3.1±0.07ms     8.1 MB/sec    1.00      2.8±0.07ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-27 19:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/outdated_version_block.rs`:79 on 2023-05-27 19:42_

What if there's an `else` in the `elif` block body? Would we not accidentally flag that as the final `else`?

---

_@JonathanPlasse reviewed on 2023-05-27 21:24_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/outdated_version_block.rs`:79 on 2023-05-27 21:24_

We need `elif` or `else` or neither, We do not need both. So, I simplified the logic.

---

_Label `bug` added by @charliermarsh on 2023-05-28 03:32_

---

_Label `autofix` added by @charliermarsh on 2023-05-28 03:32_

---

_Merged by @charliermarsh on 2023-05-28 03:37_

---

_Closed by @charliermarsh on 2023-05-28 03:37_

---

_Branch deleted on 2023-05-28 05:22_

---

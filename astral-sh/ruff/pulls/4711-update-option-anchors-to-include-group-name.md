```yaml
number: 4711
title: Update option anchors to include group name
type: pull_request
state: merged
author: jlaneve
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-dup-option-anchor
created_at: 2023-05-29T21:00:50Z
updated_at: 2023-05-29T21:47:48Z
url: https://github.com/astral-sh/ruff/pull/4711
synced_at: 2026-01-12T03:50:03Z
```

# Update option anchors to include group name

---

_Pull request opened by @jlaneve on 2023-05-29 21:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

closes https://github.com/charliermarsh/ruff/issues/4707

This PR updates the docs generation to include the group name (if it's there) in the anchor (the `#pep8-naming-ignore-names` in `/docs/settings/#pep8-naming-ignore-names`). Previously, there were issues when option names collided, as described in the linked issue.

## Test Plan

<!-- How was it tested? -->

n/a

---

_Review comment by @jlaneve on `crates/ruff_dev/src/generate_options.rs`:15 on 2023-05-29 21:03_

This uses [attr_list](https://python-markdown.github.io/extensions/attr_list/) as recommended by [mkdocs](https://github.com/mkdocs/mkdocs/issues/744).

---

_@jlaneve reviewed on 2023-05-29 21:03_

---

_Marked ready for review by @jlaneve on 2023-05-29 21:03_

---

_Merged by @charliermarsh on 2023-05-29 21:26_

---

_Closed by @charliermarsh on 2023-05-29 21:26_

---

_Comment by @charliermarsh on 2023-05-29 21:26_

This looks great, thank you :)

---

_Label `documentation` added by @charliermarsh on 2023-05-29 21:26_

---

_Comment by @github-actions[bot] on 2023-05-29 21:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.48ms     2.5 MB/sec    1.04     16.8±0.40ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.10ms     4.3 MB/sec    1.02      4.0±0.09ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.04   505.9±13.41µs     5.8 MB/sec    1.00   486.5±11.51µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.16ms     3.8 MB/sec    1.02      6.8±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.1±0.04ms     5.0 MB/sec    1.00      7.9±0.11ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1814.8±7.58µs     9.2 MB/sec    1.00  1767.0±27.77µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    206.2±1.10µs    14.3 MB/sec    1.00    204.7±4.38µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.13ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.2±0.03ms     6.6 MB/sec    1.01      6.2±0.01ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1216.1±27.27µs    13.7 MB/sec    1.02   1243.0±2.26µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    126.1±2.25µs    23.4 MB/sec    1.02    128.4±0.88µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.5 MB/sec    1.01      2.7±0.01ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     20.0±0.62ms     2.0 MB/sec     1.06     21.2±0.90ms  1968.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.2 MB/sec     1.03      5.3±0.20ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   678.4±45.23µs     4.3 MB/sec     1.00   663.5±45.42µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.31ms     2.9 MB/sec     1.00      8.8±0.24ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.03     10.6±0.67ms     3.8 MB/sec     1.00     10.3±0.37ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.10ms     8.0 MB/sec     1.04      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.6±13.56µs    11.6 MB/sec     1.14   288.8±31.33µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.22ms     5.8 MB/sec     1.07      4.7±0.19ms     5.4 MB/sec
parser/large/dataset.py                    1.03      8.1±0.16ms     5.0 MB/sec     1.00      7.9±0.27ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1556.7±191.47µs    10.7 MB/sec    1.01  1578.9±91.88µs    10.5 MB/sec
parser/numpy/globals.py                    1.00    156.9±8.51µs    18.8 MB/sec     1.05   164.5±17.08µs    17.9 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.10ms     7.2 MB/sec     1.01      3.6±0.12ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

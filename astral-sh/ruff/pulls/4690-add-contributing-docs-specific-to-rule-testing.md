```yaml
number: 4690
title: Add contributing docs specific to rule-testing patterns
type: pull_request
state: merged
author: jlaneve
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-rule-testing
created_at: 2023-05-27T23:27:15Z
updated_at: 2023-05-28T23:15:32Z
url: https://github.com/astral-sh/ruff/pull/4690
synced_at: 2026-01-12T03:50:03Z
```

# Add contributing docs specific to rule-testing patterns

---

_Pull request opened by @jlaneve on 2023-05-27 23:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds a section to the contributing guide addressing Ruff's patterns for testing rules! I wrote my first rule a few hours ago and hadn't worked with this kind of testing before, so I figured it'd be good to explicitly addressing this.

## Test Plan

<!-- How was it tested? -->

n/a


---

_Comment by @github-actions[bot] on 2023-05-27 23:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.01     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.4±3.39µs     7.0 MB/sec    1.00    423.0±0.71µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1493.8±1.60µs    11.1 MB/sec    1.00   1499.3±2.12µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.3±0.24µs    17.4 MB/sec    1.00    169.5±0.82µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.16      6.1±0.01ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1028.5±0.56µs    16.2 MB/sec    1.13   1160.8±2.60µs    14.3 MB/sec
parser/numpy/globals.py                    1.00    106.5±0.37µs    27.7 MB/sec    1.10    116.8±0.36µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.14      2.5±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.16ms     2.4 MB/sec    1.02     17.2±0.18ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.02      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.7±6.76µs     6.0 MB/sec    1.02    502.7±5.63µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.6 MB/sec    1.01      7.2±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.9 MB/sec    1.01      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.3±15.53µs     9.5 MB/sec    1.02  1797.6±22.22µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.1±3.16µs    14.9 MB/sec    1.04    206.3±4.84µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.02      3.8±0.04ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.3 MB/sec    1.01      6.5±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1233.7±24.34µs    13.5 MB/sec    1.01  1245.5±18.15µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    125.8±2.72µs    23.5 MB/sec    1.00    126.4±2.05µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.2 MB/sec    1.01      2.8±0.05ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-05-28 05:29_

---

_Review comment by @JonathanPlasse on `CONTRIBUTING.md`:154 on 2023-05-28 05:29_

If you use the script `add_rule.py` the file is created automatically.

---

_Comment by @charliermarsh on 2023-05-28 22:47_

These are great. Thanks tons, @jlaneve!

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:156 on 2023-05-28 22:47_

(Everything above is just drive-by formatting changes added by me after the initial PR.)

---

_@charliermarsh reviewed on 2023-05-28 22:47_

---

_Label `documentation` added by @charliermarsh on 2023-05-28 22:48_

---

_Merged by @charliermarsh on 2023-05-28 22:52_

---

_Closed by @charliermarsh on 2023-05-28 22:52_

---

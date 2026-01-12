```yaml
number: 6362
title: "Add `PT011` and `PT012` docs"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT011-PT012-docs
created_at: 2023-08-05T07:32:56Z
updated_at: 2023-08-07T01:30:32Z
url: https://github.com/astral-sh/ruff/pull/6362
synced_at: 2026-01-12T02:52:04Z
```

# Add `PT011` and `PT012` docs

---

_Pull request opened by @harupy on 2023-08-05 07:32_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-05 08:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.3±0.13ms     4.9 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1635.6±31.20µs    10.2 MB/sec    1.00  1625.6±20.29µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    182.8±2.53µs    16.1 MB/sec    1.00    182.0±3.16µs    16.2 MB/sec
formatter/pydantic/types.py                1.01      3.4±0.03ms     7.4 MB/sec    1.00      3.4±0.05ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.09ms     4.0 MB/sec    1.05     10.8±0.06ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.2±1.61µs     7.7 MB/sec    1.00    383.7±0.42µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.7±0.03ms     5.4 MB/sec    1.05      4.9±0.03ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.02ms     7.8 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1124.6±13.65µs    14.8 MB/sec    1.00   1123.9±3.30µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    126.7±1.69µs    23.3 MB/sec    1.00    126.6±0.39µs    23.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.01ms    10.9 MB/sec    1.00      2.4±0.02ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.0±0.46ms     3.7 MB/sec    1.00     10.7±0.42ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.23ms     7.7 MB/sec    1.02      2.2±0.17ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   242.0±20.36µs    12.2 MB/sec    1.00   241.4±17.37µs    12.2 MB/sec
formatter/pydantic/types.py                1.01      4.7±0.25ms     5.4 MB/sec    1.00      4.7±0.25ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.42ms     2.8 MB/sec    1.02     14.8±0.53ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.13ms     4.3 MB/sec    1.00      3.9±0.16ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   504.0±22.33µs     5.9 MB/sec    1.00   484.1±23.91µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.8±0.28ms     3.8 MB/sec    1.00      6.6±0.24ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.23ms     5.4 MB/sec    1.00      7.5±0.24ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1554.4±87.36µs    10.7 MB/sec    1.01  1570.8±70.54µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.05   183.9±10.18µs    16.0 MB/sec    1.00    175.9±9.72µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.16ms     7.6 MB/sec    1.00      3.3±0.16ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/raises.rs`:21 on 2023-08-05 17:30_

I know it's tedious, but do you mind wrapping these to 80 characters? It renders better in the terminal.

---

_@charliermarsh reviewed on 2023-08-05 17:30_

---

_@harupy reviewed on 2023-08-06 02:39_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/raises.rs`:21 on 2023-08-06 02:39_

sure!

---

_Label `documentation` added by @charliermarsh on 2023-08-07 01:28_

---

_Merged by @charliermarsh on 2023-08-07 01:28_

---

_Closed by @charliermarsh on 2023-08-07 01:28_

---

_@charliermarsh reviewed on 2023-08-07 01:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/raises.rs`:21 on 2023-08-07 01:28_

Thanks :)

---

_Branch deleted on 2023-08-07 01:30_

---

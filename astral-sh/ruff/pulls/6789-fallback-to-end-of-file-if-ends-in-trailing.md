```yaml
number: 6789
title: Fallback to end-of-file if ends in trailing continuation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/eof
created_at: 2023-08-22T18:43:09Z
updated_at: 2023-08-22T19:19:51Z
url: https://github.com/astral-sh/ruff/pull/6789
synced_at: 2026-01-12T02:45:38Z
```

# Fallback to end-of-file if ends in trailing continuation

---

_Pull request opened by @charliermarsh on 2023-08-22 18:43_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Given:

```python
def end_of_file():
    if False:
        return 1
    x = 2 \

```

Then when searching for the end of the `x = 2` statement, we'd reach a panic as we'd hit the last line (`\\`) and abort, since the universal iterator doesn't return trailing newlines. Instead, we should just use the end of the file as the fallback.

Closes https://github.com/astral-sh/ruff/issues/6787.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-22 18:43_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-22 18:43_

---

_Comment by @github-actions[bot] on 2023-08-22 19:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.01ms    12.3 MB/sec    1.00      3.3±0.03ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    677.1±9.63µs    24.6 MB/sec    1.00    676.4±8.72µs    24.6 MB/sec
formatter/numpy/globals.py                 1.00     71.3±0.34µs    41.4 MB/sec    1.02     72.9±3.35µs    40.5 MB/sec
formatter/pydantic/types.py                1.01  1353.6±25.80µs    18.8 MB/sec    1.00  1340.1±11.69µs    19.0 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.06ms     3.9 MB/sec    1.01     10.7±0.04ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.02ms     5.8 MB/sec    1.00      2.9±0.03ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    322.4±1.90µs     9.2 MB/sec    1.01    325.9±1.07µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.03ms     4.7 MB/sec    1.01      5.5±0.08ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.19ms     7.3 MB/sec    1.02      5.7±0.02ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1177.5±7.05µs    14.1 MB/sec    1.02   1201.6±3.32µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.4±1.53µs    23.9 MB/sec    1.02    125.3±0.54µs    23.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.02      2.5±0.01ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.8±0.12ms    10.8 MB/sec    1.00      3.7±0.16ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   762.5±19.96µs    21.8 MB/sec    1.00   759.0±16.56µs    21.9 MB/sec
formatter/numpy/globals.py                 1.02     81.2±5.13µs    36.3 MB/sec    1.00     79.4±2.38µs    37.2 MB/sec
formatter/pydantic/types.py                1.00  1514.9±25.86µs    16.8 MB/sec    1.01  1533.3±30.91µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.28ms     3.2 MB/sec    1.01     12.6±0.24ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.06ms     4.8 MB/sec    1.00      3.4±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.9±7.37µs     6.7 MB/sec    1.04   455.5±19.06µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.12ms     3.9 MB/sec    1.00      6.6±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.18ms     5.8 MB/sec    1.02      7.1±0.31ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1508.6±41.76µs    11.0 MB/sec    1.02  1534.3±137.56µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.9±3.15µs    17.0 MB/sec    1.00    174.6±2.53µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.07ms     8.1 MB/sec    1.06      3.3±0.25ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-22 19:06_

---

_Merged by @charliermarsh on 2023-08-22 19:12_

---

_Closed by @charliermarsh on 2023-08-22 19:12_

---

_Branch deleted on 2023-08-22 19:12_

---

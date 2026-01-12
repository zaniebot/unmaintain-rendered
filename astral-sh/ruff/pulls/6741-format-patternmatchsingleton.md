```yaml
number: 6741
title: "Format `PatternMatchSingleton`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-pattern-match-singleton
created_at: 2023-08-21T21:17:44Z
updated_at: 2023-08-25T20:45:16Z
url: https://github.com/astral-sh/ruff/pull/6741
synced_at: 2026-01-12T15:55:22Z
```

# Format `PatternMatchSingleton`

---

_@LaBatata101_

## Summary
There's a case where a trailing comment ins't formatted like in Black. Consider the following code:
```python
match x:
    case (
            None # trailing
            ):
        ...
```

Black formats to
```python
match x:
    case (None):  # trailing
        ...
```

And Ruff will format to
```python
match x:
    case (
        None  # trailing
    ):
        ...
```
Closes #6557

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-21 21:48_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      3.4±0.01ms    12.0 MB/sec    1.00      3.3±0.02ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.04    714.0±3.24µs    23.3 MB/sec    1.00    689.6±2.05µs    24.1 MB/sec
formatter/numpy/globals.py                 1.06     77.3±0.38µs    38.2 MB/sec    1.00     72.9±1.18µs    40.5 MB/sec
formatter/pydantic/types.py                1.03  1388.9±15.02µs    18.4 MB/sec    1.00  1350.3±23.26µs    18.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.04ms     4.0 MB/sec    1.01     10.3±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.00ms     6.1 MB/sec    1.01      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.4±1.00µs     7.6 MB/sec    1.01    395.2±0.84µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.02ms     4.8 MB/sec    1.01      5.4±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.5±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1208.8±1.61µs    13.8 MB/sec    1.00   1209.5±5.59µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.6±0.20µs    21.1 MB/sec    1.02    142.2±0.34µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      2.9±0.03ms    14.2 MB/sec    1.00      2.9±0.03ms    14.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    562.2±9.04µs    29.6 MB/sec    1.03    577.2±6.19µs    28.8 MB/sec
formatter/numpy/globals.py                 1.00     56.2±4.08µs    52.5 MB/sec    1.06     59.4±5.75µs    49.7 MB/sec
formatter/pydantic/types.py                1.00  1186.2±16.90µs    21.5 MB/sec    1.00  1187.7±27.54µs    21.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.04ms     4.1 MB/sec    1.02     10.2±0.07ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.02      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    296.0±3.58µs    10.0 MB/sec    1.02    300.8±3.90µs     9.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.03ms     5.0 MB/sec    1.02      5.2±0.04ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.02ms     7.3 MB/sec    1.01      5.6±0.03ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1137.1±10.58µs    14.6 MB/sec    1.00  1142.2±11.01µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    119.7±0.88µs    24.7 MB/sec    1.03    123.1±2.45µs    24.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.06ms    10.4 MB/sec    1.01      2.5±0.05ms    10.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-08-22 03:12_

I think for the mentioned case we should handle it appropriately (see #6750). I wouldn't consider it as a blocker for this PR as the logic for the split is in the `MatchCase` formatting.

---

_@MichaReiser approved on 2023-08-22 06:23_

---

_Label `formatter` added by @MichaReiser on 2023-08-22 06:23_

---

_Merged by @MichaReiser on 2023-08-22 06:23_

---

_Closed by @MichaReiser on 2023-08-22 06:23_

---

_Branch deleted on 2023-08-25 20:45_

---

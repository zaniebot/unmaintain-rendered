```yaml
number: 4668
title: "Implement Pylint's `yield-inside-async-function` rule (`PLE1700`) "
type: pull_request
state: merged
author: chanman3388
labels:
  - rule
assignees: []
merged: true
base: main
head: PLE1700
created_at: 2023-05-26T09:29:41Z
updated_at: 2023-05-27T01:34:40Z
url: https://github.com/astral-sh/ruff/pull/4668
synced_at: 2026-01-12T03:50:03Z
```

# Implement Pylint's `yield-inside-async-function` rule (`PLE1700`) 

---

_Pull request opened by @chanman3388 on 2023-05-26 09:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement pylint's `yield-inside-async-function` rule [E1700](https://pylint.pycqa.org/en/latest/user_guide/messages/error/yield-inside-async-function.html)
I believe since 3.6, `yield` is allowed inside an async function, but `yield from` is still not allowed hence I decided to rename it.
It should be possible to provide an autofix in _some_ situations, but I haven't looked into it yet.

## Test Plan

Made a noddy example to have a `yield` inside of a an async function, and a `yield from` inside an async function. Tested against pylint's behaviour.


---

_Comment by @github-actions[bot] on 2023-05-26 09:39_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.07ms     2.7 MB/sec    1.01     15.2±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.01      3.7±0.02ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    378.9±2.56µs     7.8 MB/sec    1.00    377.6±2.84µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1596.1±3.15µs    10.4 MB/sec    1.01   1611.2±7.63µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.5±0.28µs    16.8 MB/sec    1.02    178.8±0.49µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.02      3.4±0.03ms     7.4 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1122.1±0.51µs    14.8 MB/sec    1.00   1124.6±8.85µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    114.3±0.23µs    25.8 MB/sec    1.00    114.1±0.27µs    25.9 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.9±0.72ms  1996.9 KB/sec    1.01     21.1±0.72ms  1970.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.20ms     3.2 MB/sec    1.00      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   640.1±32.87µs     4.6 MB/sec    1.00   629.2±32.47µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.34ms     2.9 MB/sec    1.01      8.9±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.46ms     3.9 MB/sec    1.00     10.2±0.30ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.10ms     7.5 MB/sec    1.00      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.04   260.6±15.01µs    11.3 MB/sec    1.00    251.7±9.81µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.16ms     5.4 MB/sec    1.00      4.7±0.28ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.0±0.21ms     5.1 MB/sec    1.02      8.1±0.21ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.02  1555.1±85.57µs    10.7 MB/sec    1.00  1526.4±59.69µs    10.9 MB/sec
parser/numpy/globals.py                    1.02   160.3±12.00µs    18.4 MB/sec    1.00    156.4±7.14µs    18.9 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.13ms     7.4 MB/sec    1.01      3.5±0.11ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:3088 on 2023-05-26 12:16_

I was confused about why the rule does not check whether `expr` is a `YieldFrom` expression. I think we can make this clearer by only accepting a `ExprYieldFrom` in `yield_from_in_async_function` and passing the `yield_from` expression here.


1. Change the match arm to `Expr::YieldFrom(yield_from) => {`

```suggestion
                    pylint::rules::yield_from_in_async_function(self, yield_from);
```

---

_@MichaReiser reviewed on 2023-05-26 12:16_

---

_Label `rule` added by @charliermarsh on 2023-05-27 01:01_

---

_Merged by @charliermarsh on 2023-05-27 01:14_

---

_Closed by @charliermarsh on 2023-05-27 01:14_

---

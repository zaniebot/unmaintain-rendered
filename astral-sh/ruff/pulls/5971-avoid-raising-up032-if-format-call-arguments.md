```yaml
number: 5971
title: "Avoid raising `UP032` if `format` call arguments contain multiline expressions"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: UP032-multiline
created_at: 2023-07-22T08:02:52Z
updated_at: 2023-07-22T13:38:44Z
url: https://github.com/astral-sh/ruff/pull/5971
synced_at: 2026-01-12T03:30:22Z
```

# Avoid raising `UP032` if `format` call arguments contain multiline expressions

---

_Pull request opened by @harupy on 2023-07-22 08:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix a regression introduced by https://github.com/astral-sh/ruff/pull/5638. A multiline expression can't be safely inserted into a format field.

### Example

```
> cat a.py
"{}".format(
    [
        1,
        2,
        3,
    ]
)

> cargo run -p ruff_cli -- check a.py --no-cache --select UP032 --fix
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff check a.py --no-cache --select UP032 --fix`
error: Autofix introduced a syntax error in `a.py` with rule codes UP032: EOL while scanning string literal at byte offset 5
---
f"{[
        1,
        2,
        3,
    ]}"

---
a.py:1:1: UP032 Use f-string instead of `format` call
Found 1 error.
```


## Test Plan

<!-- How was it tested? -->


New test cases

---

_Comment by @github-actions[bot] on 2023-07-22 08:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.04ms     4.2 MB/sec    1.07     10.5±0.04ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1898.9±4.89µs     8.8 MB/sec    1.06      2.0±0.01ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    208.1±0.25µs    14.2 MB/sec    1.03    214.0±1.67µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.06      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.06ms     3.0 MB/sec    1.01     13.7±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.0±1.41µs     7.9 MB/sec    1.00    376.1±1.32µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.04ms     5.8 MB/sec    1.01      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1426.1±3.55µs    11.7 MB/sec    1.02   1452.1±4.59µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.8±0.30µs    19.6 MB/sec    1.01    152.0±0.50µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.1 MB/sec    1.02      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     16.4±0.55ms     2.5 MB/sec    1.02     16.8±0.53ms     2.4 MB/sec
formatter/numpy/ctypeslib.py               1.02      3.2±0.13ms     5.3 MB/sec    1.00      3.1±0.10ms     5.4 MB/sec
formatter/numpy/globals.py                 1.00   347.9±19.18µs     8.5 MB/sec    1.02   355.3±31.78µs     8.3 MB/sec
formatter/pydantic/types.py                1.03      7.0±0.23ms     3.6 MB/sec    1.00      6.8±0.29ms     3.7 MB/sec
linter/all-rules/large/dataset.py          1.00     23.1±0.92ms  1801.5 KB/sec    1.03     23.9±0.66ms  1741.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.19ms     2.9 MB/sec    1.09      6.2±0.22ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   680.2±37.96µs     4.3 MB/sec    1.03   699.8±28.88µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.35ms     2.6 MB/sec    1.09     10.7±0.36ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.31ms     3.6 MB/sec    1.10     12.6±0.30ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.07ms     7.0 MB/sec    1.04      2.5±0.12ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   285.7±15.17µs    10.3 MB/sec    1.02    292.2±9.86µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.17ms     5.0 MB/sec    1.05      5.3±0.41ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-22 13:37_

---

_Closed by @charliermarsh on 2023-07-22 13:37_

---

_Label `bug` added by @charliermarsh on 2023-07-22 13:37_

---

_Branch deleted on 2023-07-22 13:38_

---

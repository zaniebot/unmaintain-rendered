```yaml
number: 5158
title: format StmtBreak
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: format_break
created_at: 2023-06-17T07:52:04Z
updated_at: 2023-07-07T20:48:23Z
url: https://github.com/astral-sh/ruff/pull/5158
synced_at: 2026-01-12T15:55:17Z
```

# format StmtBreak

---

_@davidszotten_

## Summary

format `StmtBreak`

trying to learn how to help out with the formatter. starting simple

## Test Plan

new snapshot test


---

_@konstin approved on 2023-06-17 08:03_

thanks

---

_Comment by @github-actions[bot] on 2023-06-17 08:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1397.9±1.97µs    11.9 MB/sec    1.00   1400.6±2.58µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    136.8±0.33µs    21.6 MB/sec    1.01    138.5±0.32µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.00ms     9.2 MB/sec    1.01      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.02     14.2±0.06ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    369.0±1.19µs     8.0 MB/sec    1.00    363.2±0.82µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.03      7.5±0.06ms     5.4 MB/sec    1.00      7.3±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1570.1±3.32µs    10.6 MB/sec    1.00   1533.2±3.95µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    168.5±0.16µs    17.5 MB/sec    1.00    165.7±0.36µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.01      7.9±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1574.9±10.08µs    10.6 MB/sec    1.01  1583.4±10.25µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    155.8±1.46µs    18.9 MB/sec    1.01    156.8±2.23µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     8.0 MB/sec    1.01      3.2±0.02ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.07ms     2.6 MB/sec    1.01     16.0±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.01      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.5±8.35µs     6.9 MB/sec    1.00    429.4±5.24µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.02      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.05ms     4.9 MB/sec    1.01      8.4±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1742.1±12.50µs     9.6 MB/sec    1.00   1732.9±9.27µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.6±1.34µs    15.8 MB/sec    1.00    186.6±1.24µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @davidszotten on 2023-06-17 08:13_

do i need any custom comment handling like in https://github.com/astral-sh/ruff/blob/30574e1a196912baaa94620fc217f53b647c41b5/crates/ruff_python_formatter/src/statement/stmt_while.rs#L21  ? if not, how come it's needed for `while`?

---

_Comment by @konstin on 2023-06-17 08:31_

`FormatNodeRule` has default implementations for `fmt_leading_comments`, `fmt_trailing_comments` and `fmt_dangling_comments` which do the right thing for `break` (a comment can be on its own line before the `break` or trailing the `break` either on the same line or on its own line line after).

For `while` it's more complicated, e.g. you can have

```python
while x: # trailing condition comment, do not indent
    body # trailing body comment, keep with body
    # trailing body comment, keep with body
# leading else comment, keep with else
else:  # trailing else comment, do not indent either
    print("else")
```

Since our AST is often lacking nodes for parts of the syntax, we often have to set comments as "dangling" in placements.rs and handle them manually in the node formatting. For the `while` case there is e.g. no node for the `else`, we get the or body as a `Vec<Stmt>`. That means we get comments once for the entire while-else statement and have to parse out the leading else comments. Similarly the `:` doesn't have any representation, so we mark trailing comments after a colon [here](https://github.com/astral-sh/ruff/blob/66089e1a2e4b8b6ad6de3dc9543cd9a8d2a764e3/crates/ruff_python_formatter/src/comments/placement.rs#L576). 

---

_Merged by @konstin on 2023-06-17 08:31_

---

_Closed by @konstin on 2023-06-17 08:31_

---

_Comment by @davidszotten on 2023-06-17 10:04_

thank you, i sort of followed that. will hopefully become more clear as i dig in more

---

_Comment by @charliermarsh on 2023-06-17 14:38_

Thanks @davidszotten!

---

_Branch deleted on 2023-07-07 20:48_

---

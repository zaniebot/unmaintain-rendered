```yaml
number: 5134
title: Add support for global and nonlocal symbol renames
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rename-globals
created_at: 2023-06-16T02:30:03Z
updated_at: 2023-06-16T14:51:46Z
url: https://github.com/astral-sh/ruff/pull/5134
synced_at: 2026-01-12T15:55:17Z
```

# Add support for global and nonlocal symbol renames

---

_@charliermarsh_

## Summary

In #5074, we introduced an abstraction to support local symbol renames ("local" here refers to "within a module"). However, that abstraction didn't support `global` and `nonlocal` symbols. This PR extends it to those cases.

Broadly, there are considerations.

First, if we're renaming a symbol in a scope in which it is declared `global` or `nonlocal`. For example, given:

```python
x = 1

def foo():
    global x
```

Then when renaming `x` in `foo`, we need to detect that it's `global` and instead perform the rename starting from the module scope.

Second, when renaming a symbol, we need to determine the scopes in which it is declared `global` or `nonlocal`. This is effectively the inverse of the above: when renaming `x` in the module scope, we need to detect that we should _also_ rename `x` in `foo`.

To support these cases, the renaming algorithm was adjusted as follows:

- When we start a rename in a scope, determine whether the symbol is declared `global` or `nonlocal` by looking for a `global` or `nonlocal` binding. If it is, start the rename in the defining scope. (This requires storing the defining scope on the `nonlocal` binding, which is new.)
- We then perform the rename in the defining scope.
- We then check whether the symbol was declared as `global` or `nonlocal` in any scopes, and perform the rename in those scopes too. (Thankfully, this doesn't need to be done recursively.)

Closes #5092.

## Test Plan

Added some additional snapshot tests.

---

_Comment by @github-actions[bot] on 2023-06-16 02:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.02ms     5.9 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1408.6±22.44µs    11.8 MB/sec    1.00   1412.2±4.40µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    139.2±0.67µs    21.2 MB/sec    1.00    138.9±0.57µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.01      2.8±0.02ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.08ms     2.9 MB/sec    1.02     14.4±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.4±0.67µs     8.0 MB/sec    1.01    372.9±0.98µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.06ms     4.2 MB/sec    1.02      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.05ms     5.7 MB/sec    1.02      7.3±0.03ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1501.6±4.21µs    11.1 MB/sec    1.02   1538.4±6.34µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.16µs    18.1 MB/sec    1.02    165.7±0.62µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.02      3.3±0.02ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.7±0.34ms     4.2 MB/sec    1.00      9.5±0.32ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.02  1969.4±82.41µs     8.5 MB/sec    1.00  1933.6±113.85µs     8.6 MB/sec
formatter/numpy/globals.py                 1.01   190.8±11.68µs    15.5 MB/sec    1.00   189.0±11.00µs    15.6 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.18ms     6.4 MB/sec    1.00      3.9±0.17ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±0.69ms     2.0 MB/sec    1.01     20.1±0.54ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.21ms     3.2 MB/sec    1.00      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   618.6±29.03µs     4.8 MB/sec    1.01   622.7±32.89µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.0±0.48ms     2.8 MB/sec    1.00      8.9±0.38ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.33ms     4.0 MB/sec    1.00     10.1±0.34ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.09ms     7.7 MB/sec    1.00      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.4±13.45µs    12.1 MB/sec    1.04   253.0±14.06µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.19ms     5.5 MB/sec    1.00      4.6±0.16ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-06-16 02:59_

---

_@konstin approved on 2023-06-16 11:03_

Code looks good as far as i can judge

I found a fun new error while trying out semantics: `SyntaxError: name 'y' is used prior to global declaration`

```python
y = 1
def c():
    y = 2
    print(y)
    global y
    print(y)
```

---

_Merged by @charliermarsh on 2023-06-16 14:35_

---

_Closed by @charliermarsh on 2023-06-16 14:35_

---

_Branch deleted on 2023-06-16 14:35_

---

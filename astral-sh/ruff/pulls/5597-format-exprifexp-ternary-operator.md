```yaml
number: 5597
title: " Format ExprIfExp (ternary operator)"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: format_expr_if_exp
created_at: 2023-07-07T16:39:58Z
updated_at: 2023-07-08T09:45:45Z
url: https://github.com/astral-sh/ruff/pull/5597
synced_at: 2026-01-12T03:36:55Z
```

#  Format ExprIfExp (ternary operator)

---

_Pull request opened by @konstin on 2023-07-07 16:39_

## Summary

Format `ExprIfExp`, also known as the ternary operator or inline `if`. It can look like
```python
a1 = 1 if True else 2
```
but also
```python
b1 = (
    # We return "a" ...
    "a" # that's our True value
    # ... if this condition matches ...
    if True # that's our test
    # ... otherwise we return "b§
    else "b" # that's our False value
)
```

This also fixes a visitor order bug.

The jaccard index on django goes from 0.911 to 0.915.

## Test Plan

I added fixtures without and with comments in strange places. 


---

_Comment by @github-actions[bot] on 2023-07-07 16:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.16ms     3.9 MB/sec    1.01     10.5±0.25ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.3±0.05ms     7.3 MB/sec    1.00      2.2±0.07ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    262.3±8.84µs    11.3 MB/sec    1.03   269.1±34.82µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.12ms     5.1 MB/sec    1.00      5.0±0.14ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     18.4±0.62ms     2.2 MB/sec    1.03     18.9±0.59ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.13ms     3.8 MB/sec    1.02      4.5±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   561.2±18.73µs     5.3 MB/sec    1.04   583.8±19.25µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.25ms     3.2 MB/sec    1.01      8.1±0.15ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.24ms     4.5 MB/sec    1.02      9.2±0.23ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1976.8±49.64µs     8.4 MB/sec    1.00  1950.9±37.93µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.03   236.6±14.34µs    12.5 MB/sec    1.00   230.3±10.27µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.10ms     6.2 MB/sec    1.00      4.1±0.11ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09     12.7±0.47ms     3.2 MB/sec    1.00     11.7±0.52ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.13ms     6.4 MB/sec    1.01      2.6±0.14ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   293.4±17.23µs    10.1 MB/sec    1.06   311.0±15.72µs     9.5 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.27ms     4.3 MB/sec    1.00      5.9±0.21ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.00     22.1±0.85ms  1881.2 KB/sec    1.00     22.1±0.88ms  1886.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.7±0.18ms     2.9 MB/sec    1.00      5.6±0.23ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   677.6±40.28µs     4.4 MB/sec    1.01   682.4±49.26µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.6±0.48ms     2.6 MB/sec    1.00      9.5±0.44ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.43ms     3.7 MB/sec    1.02     11.1±0.37ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10      2.3±0.08ms     7.1 MB/sec    1.00      2.1±0.10ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.02   286.3±15.43µs    10.3 MB/sec    1.00   280.7±30.19µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.9±0.18ms     5.2 MB/sec    1.00      4.7±0.23ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/preorder.rs`:481 on 2023-07-07 17:03_

Some Python noob must have implemented the `PreOrder` visitor, assuming its the same as in JavaScript ;)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1173 on 2023-07-07 17:04_

```suggestion
/// This placement ensures comments remain in their previous order. This edge case only
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1215 on 2023-07-07 17:06_

It should be possible to add support for `if` and `else`. Comments are already `non-single` token strings. 
The backwards lexing is a bit more annoying but should be doable as well. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1197 on 2023-07-07 17:07_

I recommend moving the `line_position` check to the very top to avoid unnecessarily lexing own line comments.

---

_@MichaReiser approved on 2023-07-07 17:08_

> The jaccard index on django afterwards is 0.915.

What was the value before?


I think it could make sense to give it a quick try to add the `if` and `else` tokens to the `SimpleTokenizer`. 

---

_Comment by @konstin on 2023-07-07 19:02_

> What was the value before?

0.911, added it to the description

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1197 on 2023-07-07 19:03_

good point

---

_@konstin reviewed on 2023-07-07 19:03_

---

_@konstin reviewed on 2023-07-07 19:05_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1215 on 2023-07-07 19:05_

i didn't want to change the single-char-ness of the tokenizer just for this PR, but if you want i can do the refactoring in another PR

---

_Comment by @konstin on 2023-07-07 19:06_

> I think it could make sense to give it a quick try to add the if and else tokens to the SimpleTokenizer.

I add it to my TODO list

---

_Comment by @davidszotten on 2023-07-07 19:08_

> > I think it could make sense to give it a quick try to add the if and else tokens to the SimpleTokenizer.
> 
> I add it to my TODO list

ooh, maybe `in` and `if` could be useful for for  #5600

i hadn't looked in detail but didn't consider it since it only has "special" chars so far, no words

---

_Merged by @konstin on 2023-07-07 19:11_

---

_Closed by @konstin on 2023-07-07 19:11_

---

_Branch deleted on 2023-07-07 19:11_

---

_Comment by @davidszotten on 2023-07-08 09:45_

> > I think it could make sense to give it a quick try to add the if and else tokens to the SimpleTokenizer.
> 
> I add it to my TODO list

i took a stab at it: #5610

---

```yaml
number: 5121
title: "[`perflint`] Add `PERF101` with autofix"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PER101
created_at: 2023-06-15T14:12:02Z
updated_at: 2023-06-22T21:36:15Z
url: https://github.com/astral-sh/ruff/pull/5121
synced_at: 2026-01-12T03:36:54Z
```

# [`perflint`] Add `PERF101` with autofix

---

_Pull request opened by @qdegraaf on 2023-06-15 14:12_

## Summary

Adds PERF101 which checks for unnecessary casts to `list` in for loops. 

NOTE: Is not fully equal to its upstream implementation as this implementation does not flag based on type annotations
(i.e.):
```python
def foo(x: List[str]):
    for y in list(x):
        ...
```

With the current set-up it's quite hard to get the annotation from a function arg from its binding. Problem is best considered broader than this implementation.

## Test Plan

Added fixture. 

## Issue links

Refers: https://github.com/astral-sh/ruff/issues/4789 


---

_Comment by @github-actions[bot] on 2023-06-15 14:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1748.9±2.94µs     9.5 MB/sec    1.01   1757.8±9.11µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    195.7±1.44µs    15.1 MB/sec    1.00    195.6±0.46µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.02ms     6.2 MB/sec    1.00      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.10ms     2.5 MB/sec    1.00     16.4±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.04ms     4.0 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    519.7±1.84µs     5.7 MB/sec    1.00    512.8±1.27µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.04ms     3.5 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.03ms     4.9 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1803.5±4.41µs     9.2 MB/sec    1.00   1764.7±3.90µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    200.7±0.45µs    14.7 MB/sec    1.00    197.2±0.47µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.8±0.02ms     6.7 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      7.9±0.15ms     5.1 MB/sec     1.00      7.9±0.18ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1703.4±53.99µs     9.8 MB/sec     1.00  1693.2±45.56µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    185.0±5.26µs    16.0 MB/sec     1.04   192.4±16.40µs    15.3 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.11ms     6.5 MB/sec     1.00      4.0±0.08ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.45ms     2.6 MB/sec     1.00     15.5±0.35ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec     1.00      4.1±0.10ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.4±9.33µs     6.0 MB/sec     1.01   497.1±14.84µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.17ms     3.7 MB/sec     1.01      7.0±0.20ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.15ms     5.0 MB/sec     1.00      8.0±0.15ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1740.6±104.00µs     9.6 MB/sec    1.00  1713.3±45.69µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    197.9±8.50µs    14.9 MB/sec     1.00    192.6±5.16µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.11ms     6.9 MB/sec     1.00      3.6±0.09ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@qdegraaf reviewed on 2023-06-21 15:22_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 15:22_

If there's a more elegant way to extract the string representation of the value in the `list()` call to replace the whole thing instead of messing about with set ranges, I'm happy for the input.

---

_Marked ready for review by @qdegraaf on 2023-06-21 15:22_

---

_@MichaReiser reviewed on 2023-06-21 19:18_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 19:18_

Can you use the statt of the iterator? It has the downside that it truncates comments but produces a correct fix if there's a space (or even comment between list and the (

---

_@qdegraaf reviewed on 2023-06-21 20:55_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 20:55_

Sure! I'm not sure I exactly understand what you mean, though. Can you maybe give an example that would break in this implementation that I can add to the fixture to check after I have made the tweak if I understood?

---

_@qdegraaf reviewed on 2023-06-21 21:48_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 21:48_

Because the range being passed in now is already the range of the `iter: &Expr` iterator. And if you mean the iterable then I don't really see how that helps with comments (aside from the issue of getting the range from the iterable depending on whether it's a binding or literal etc.)


---

_@qdegraaf reviewed on 2023-06-21 21:56_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 21:56_

Ah wait I think I get it. Do you mean taking the deletion range to be from the start of the iterator to the start of the iterable? 

Now that I'm looking it over that still leaves another scenario that would be unfixable namely:
```python
for bla in list(
    {1, 2, 3}
)
```

The most complete way would be to somehow extract whatever is in the `list()` call in its entirety, trim the whitespace, and replace the whole range of the iterator with that value. That would catch multiline args but would still leave the issue of comments in something like:

```python
for i in list(
    # bla bla
    {1, 2, 3}
): 
    pass
```

I'll see if I can figure something out tomorrow and otherwise I might leave the autofix for later

---

_@MichaReiser reviewed on 2023-06-21 22:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 22:12_

You would probably need to use libCST if you want to retain comments. Many of Ruff's fixes do not handle comments.

I think it's otherwise fine to delete the ranges from the `list(` call expression start to the start of the iterable and from the end of the iterable to the end of the `list call`

```python
for bla in list(   { 1, 2, 3 }  ):
           ^^^^^^^^           ^^^
	...
```

---

_@qdegraaf reviewed on 2023-06-21 22:25_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/perflint/rules/unnecessary_list_cast.rs`:95 on 2023-06-21 22:25_

Ah in that case I'll just delete the comments. Thanks for the feedback, pushed change!

---

_Renamed from "`[perflint]` Add PERF101 with autofix" to "`[perflint]` Add `PERF101` with autofix" by @qdegraaf on 2023-06-22 19:34_

---

_Label `rule` added by @charliermarsh on 2023-06-22 20:32_

---

_Renamed from "`[perflint]` Add `PERF101` with autofix" to "[`perflint`] Add `PERF101` with autofix" by @charliermarsh on 2023-06-22 20:32_

---

_Merged by @charliermarsh on 2023-06-22 20:44_

---

_Closed by @charliermarsh on 2023-06-22 20:44_

---

_Branch deleted on 2023-06-22 21:36_

---

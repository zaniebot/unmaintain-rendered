```yaml
number: 5221
title: Fix AST visitor traversal order
type: pull_request
state: merged
author: jgberry
labels: []
assignees: []
merged: true
base: main
head: fix-ast-visitor-argument-order
created_at: 2023-06-20T17:49:33Z
updated_at: 2023-06-21T20:05:47Z
url: https://github.com/astral-sh/ruff/pull/5221
synced_at: 2026-01-12T03:43:30Z
```

# Fix AST visitor traversal order

---

_Pull request opened by @jgberry on 2023-06-20 17:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

According to the AST visitor documentation, the AST visitor "visits all nodes in the AST recursively in evaluation-order". However, the current traversal fails to meet this specification in a few places.

### Function traversal

```python
order = []
@(order.append("decorator") or (lambda x: x))
def f(
    posonly: order.append("posonly annotation") = order.append("posonly default"),
    /,
    arg: order.append("arg annotation") = order.append("arg default"),
    *args: order.append("vararg annotation"),
    kwarg: order.append("kwarg annotation") = order.append("kwarg default"),
    **kwargs: order.append("kwarg annotation")
) -> order.append("return annotation"):
    pass
print(order)
```

Executing the above snippet using CPython 3.10.6 prints the following result (formatted for readability):
```python
[
    'decorator',
    'posonly default',
    'arg default',
    'kwarg default',
    'arg annotation',
    'posonly annotation',
    'vararg annotation',
    'kwarg annotation',
    'kwarg annotation',
    'return annotation',
]
```

Here we can see that decorators are evaluated first, followed by argument defaults, and annotations are last. The current traversal of a function's AST does not align with this order.

### Annotated assignment traversal
```python
order = []
x: order.append("annotation") = order.append("expression")
print(order)
```

Executing the above snippet using CPython 3.10.6 prints the following result:
```python
['expression', 'annotation']
```

Here we can see that an annotated assignments annotation gets evaluated after the assignment's expression. The current traversal of an annotated assignment's AST does not align with this order.

## Why?

I'm slowly working on #3946 and porting over some of the logic and tests from ssort. ssort is very sensitive to AST traversal order, so ensuring the utmost correctness here is important.

## Test Plan

There doesn't seem to be existing tests for the AST visitor, so I didn't bother adding tests for these very subtle changes. However, this behavior will be captured in the tests for the PR which addresses #3946.

---

_Comment by @charliermarsh on 2023-06-20 17:55_

Nice, thank you -- this makes sense.

We actually end up overriding this behavior in the core AST checker (which implements the `Visitor` trait), because we evaluate the defaults vs. the arguments in different scopes. The defaults are evaluated in the enclosing scope, while the arguments are evaluated within the function scope (since they inject new symbols into the scope). So, that's at least one reason why we may not have noticed this (and it's not a risky change).

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-20 18:00_

I wonder if we instead need to change how we walk over these such that, in `walk_arguments`, we walk over the defaults, then the arguments... We may want to remove `visit_arg_with_default` from the interface entirely. 🤔 

---

_@charliermarsh reviewed on 2023-06-20 18:00_

---

_Comment by @github-actions[bot] on 2023-06-20 18:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.01ms     6.4 MB/sec    1.00      6.4±0.01ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1357.4±3.57µs    12.3 MB/sec    1.00   1347.1±3.96µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    132.6±0.16µs    22.3 MB/sec    1.00    133.1±0.16µs    22.2 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.01ms     9.6 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.05ms     3.1 MB/sec    1.02     13.2±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.9±0.80µs     7.0 MB/sec    1.01    427.9±0.64µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.01      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1451.8±2.15µs    11.5 MB/sec    1.03   1489.9±2.87µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.2±0.29µs    17.6 MB/sec    1.02    171.0±0.39µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.02      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.10ms     5.2 MB/sec    1.00      7.8±0.11ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1584.0±18.99µs    10.5 MB/sec    1.01  1601.2±27.46µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    152.6±4.54µs    19.3 MB/sec    1.03    157.8±6.44µs    18.7 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.07ms     8.0 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.02     16.1±0.22ms     2.5 MB/sec    1.00     15.8±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.3±0.11ms     3.9 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.5±7.59µs     5.9 MB/sec    1.00    500.5±8.64µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.18ms     3.5 MB/sec    1.00      7.0±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.15ms     4.9 MB/sec    1.00      8.1±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1737.3±26.86µs     9.6 MB/sec    1.00  1738.1±23.87µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.1±3.61µs    15.0 MB/sec    1.01    198.8±5.90µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     6.9 MB/sec    1.00      3.7±0.07ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@jgberry reviewed on 2023-06-20 18:35_

---

_Review comment by @jgberry on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-20 18:35_

Yeah, that's the way I originally implemented it, but after resolving the conflicts from #5192, this seemed to be the simplest way to do things. I'm open to restructuring, but it would make this PR more involved.  

---

_Renamed from "AST visitor: walk arguments in correct order" to "Fix AST visitor traversal order" by @jgberry on 2023-06-20 21:43_

---

_Comment by @jgberry on 2023-06-20 21:45_

I've updated this PR to be more a general "fix-all" for a few different traversal order issues I've run into while working on #3946. The PR summary has been updated to reflect these changes.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-21 16:57_

This won't necessarily visit in the described order though. Given:

```python
def f(
  x: int = 1,
  y : float = 2.0,
):
  ...
```

This would visit `1`, then `int`, then `2.0`, then `float`. I thought you were looking to enforce `1`, then `2.0`, then `int`, then `float`. Is this change sufficient? I'm fine with either but the latter seems "more correct".

---

_@charliermarsh reviewed on 2023-06-21 16:57_

---

_@jgberry reviewed on 2023-06-21 17:49_

---

_Review comment by @jgberry on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-21 17:49_

Yes, you're right. This doesn't completely solve the issue. But, given as given as function arguments are now walked one by one, it wouldn't be possible to traverse them in the correct order without removing or avoiding calling `visit_arg_with_default`. I think the simplest solution would be to have `walk_arguments` handle all argument traversal and never call `visit_arg_with_default`, but this isn't the cleanest in my opinion. I'll throw this change in though and you can let me know what you think.

---

_@jgberry reviewed on 2023-06-21 18:37_

---

_Review comment by @jgberry on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-21 18:37_

Hmm, this became unbearably messy for my tastes. The problem is we need to call `visit_arg` with a `ArgWithDefault`, so we have to convert an `ArgWithDefault` to just a plain `Arg`... but then there are lifetime issues to resolve and the whole thing blows up. I'm inclined to leave this as is for now. Yes, it's not completely correct, but it's so subtle that I don't anticipate it causing issues.

---

_@charliermarsh reviewed on 2023-06-21 18:40_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:639 on 2023-06-21 18:40_

Hah, ok -- that's fine, lets see how it goes, this is clearly an improvement.

---

_Merged by @charliermarsh on 2023-06-21 18:40_

---

_Closed by @charliermarsh on 2023-06-21 18:40_

---

_Comment by @charliermarsh on 2023-06-21 18:41_

Thanks! I love the PR summary too, very clear.

---

_Branch deleted on 2023-06-21 20:05_

---

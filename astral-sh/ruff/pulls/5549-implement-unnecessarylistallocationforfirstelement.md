```yaml
number: 5549
title: "Implement `UnnecessaryListAllocationForFirstElement`"
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - rule
assignees: []
merged: true
base: main
head: 5503_first_list_item
created_at: 2023-07-06T01:03:53Z
updated_at: 2023-07-10T16:52:02Z
url: https://github.com/astral-sh/ruff/pull/5549
synced_at: 2026-01-12T03:36:55Z
```

# Implement `UnnecessaryListAllocationForFirstElement`

---

_Pull request opened by @evanrittenhouse on 2023-07-06 01:03_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #5503. Ready for final review as the `mkdocs` issue involving SSH keys is fixed.

Note that this will only throw on a `Name` - it will be refactorable once we have a type-checker. This means that this is the only sort of input that will throw.
```python
x = range(10)
list(x)[0]
```

I thought it'd be confusing if we supported direct function results. Consider this example, assuming we support direct results:
```python
# throws
list(range(10))[0]

def createRange(bound):
    return range(bound)

# "why doesn't this throw, but a direct `range(10)` call does?"
list(createRange(10))[0]
```
If it's necessary, I can go through the list of built-ins and find those which produce iterables, then add them to the throwing list. 

## Test Plan

Added a new fixture, then ran `cargo t`

---

_Comment by @github-actions[bot] on 2023-07-06 01:41_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>scikit-build (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/main/skbuild/setuptools_wrap.py#L248'>skbuild/setuptools_wrap.py:248:17:</a> RUF015 [*] Prefer `next(iter(plat_names))` over `list(plat_names)[0]`
</pre>

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/actions/streams.py#L435'>zerver/actions/streams.py:435:39:</a> RUF015 [*] Prefer `next(iter(altered_user_ids))` over `list(altered_user_ids)[0]`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF015 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.01ms     4.8 MB/sec    1.00      8.4±0.01ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1812.9±5.91µs     9.2 MB/sec    1.00   1805.3±4.70µs     9.2 MB/sec
formatter/numpy/globals.py                 1.01    199.6±0.66µs    14.8 MB/sec    1.00    197.9±0.38µs    14.9 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.02ms     2.9 MB/sec    1.01     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.8±1.41µs     8.0 MB/sec    1.00    365.1±1.25µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.02      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.9±4.55µs    11.3 MB/sec    1.00   1473.1±1.69µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    158.4±0.26µs    18.6 MB/sec    1.00    157.3±0.29µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.07ms     4.2 MB/sec    1.00      9.6±0.06ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.02ms     8.2 MB/sec    1.00      2.0±0.02ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    220.5±2.48µs    13.4 MB/sec    1.00    221.6±5.05µs    13.3 MB/sec
formatter/pydantic/types.py                1.02      4.7±0.04ms     5.4 MB/sec    1.00      4.6±0.02ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.01     15.9±0.11ms     2.6 MB/sec    1.00     15.8±0.26ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.9±6.02µs     6.8 MB/sec    1.00    431.1±5.81µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.09ms     3.6 MB/sec    1.00      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.01      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1681.4±11.77µs     9.9 MB/sec    1.01  1696.9±18.50µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    182.0±5.47µs    16.2 MB/sec    1.00    179.0±1.46µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     6.9 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-07-06 03:22_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2214 on 2023-07-06 12:30_

Nit: We could pass the whole subscript instead of each individual field (also cheaper, because it passes a single reference instead of 3)
```suggestion
                    ruff::rules::unnecessary_list_allocation_for_first_element(
                        self, subscript
                    );
```


And on line 2142:

```rust
   ExprSubscript(subscript @ ast::ExprSubscript { ... })
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:140 on 2023-07-06 12:33_

Do you fall back to `0` here because any non int constants are not valid subscript indexes?

---

_@MichaReiser reviewed on 2023-07-06 12:33_

---

_@evanrittenhouse reviewed on 2023-07-06 12:42_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:140 on 2023-07-06 12:42_

Yep

---

_@evanrittenhouse reviewed on 2023-07-06 12:47_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:140 on 2023-07-06 12:47_

@MichaReiser should I do something else instead?

---

_@sbrugman reviewed on 2023-07-06 14:45_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:13 on 2023-07-06 14:45_

Missing markdown header (`##`)

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:13 on 2023-07-06 16:39_

Good catch, thanks 

---

_@evanrittenhouse reviewed on 2023-07-06 16:39_

---

_Review comment by @hoxbro on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF015_RUF015.py.snap`:40 on 2023-07-06 18:00_

This fix is not correct. A slicing will return a list with the first value in it, where `next(iter(..))` will return the first value.

``` python
x = range(10)
assert list(x)[:1] == [0]
assert next(iter(x)) == 0
```

---

_@hoxbro reviewed on 2023-07-06 18:00_

---

_@evanrittenhouse reviewed on 2023-07-06 18:55_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF015_RUF015.py.snap`:40 on 2023-07-06 18:55_

Thanks, fixed.

---

_Converted to draft by @evanrittenhouse on 2023-07-06 20:48_

---

_Marked ready for review by @evanrittenhouse on 2023-07-06 22:09_

---

_Comment by @evanrittenhouse on 2023-07-08 03:16_

@charliermarsh was wondering if I could grab a review on this so I can avoid needing to rename in case `RUF015` gets taken soon :) 

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-08 19:05_

---

_Comment by @charliermarsh on 2023-07-08 19:17_

Haven't reviewed the code yet (but I will soon), but note that this will introduce at least one behavior change, which is that the exception thrown in the empty case is different:

```python
>>> next(iter([]))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> [][0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```


---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/ruff/RUF015.py`:4 on 2023-07-09 08:45_

Do you think we should support `tuple` as well?

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF015_RUF015.py.snap`:40 on 2023-07-09 08:46_

This is neat!

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/ruff/RUF015.py`:16 on 2023-07-09 08:48_

I think we could also support generators inside list such as `list(i for i in x)[0]`. What do you think?

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:56 on 2023-07-09 08:52_

nit: I would prefer to have the fix title to be short and actionable like: 
```suggestion
            "Replace with `next(iter({}))`",
```

https://github.com/search?q=repo:astral-sh/ruff+%22Replace+with&type=code

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:197 on 2023-07-09 09:00_

nit: Do we need a separate function for this? Also, I don't think you need to add a type annotation here.

```suggestion
    matches!(i, Some(0))
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:164 on 2023-07-09 09:06_

Do we need to test whether we're in a slice or not? Are there any examples where the node is `Expr::Slice` but in code it's something else?

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:148 on 2023-07-09 09:10_

Do you think we could use `enum` instead of `bool`s as otherwise it becomes a bit difficult to read the code without checking the documentation. Or, maybe even a struct with 2 fields.

---

_@dhruvmanila reviewed on 2023-07-09 09:10_

---

_@evanrittenhouse reviewed on 2023-07-09 13:34_

---

_Review comment by @evanrittenhouse on `crates/ruff/resources/test/fixtures/ruff/RUF015.py`:4 on 2023-07-09 13:34_

I thought about it, but wasn't sure since the original post talked about `list()` only. I can add it in though.

---

_Comment by @evanrittenhouse on 2023-07-09 13:39_

> Haven't reviewed the code yet (but I will soon), but note that this will introduce at least one behavior change, which is that the exception thrown in the empty case is different:

@charliermarsh Yeah, I figured it'd be fine since it's a suggested fix. I don't think there's a way to get around it though, is there?

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:164 on 2023-07-09 13:40_

@dhruvmanila sorry, could you please provide an example? A bit unclear on what you mean

---

_@evanrittenhouse reviewed on 2023-07-09 13:40_

---

_@dhruvmanila reviewed on 2023-07-10 04:12_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:164 on 2023-07-10 04:12_

Sorry for being unclear. Here, I'm assuming you're checking whether the node is a slice element (`[1:]`) as oppose to a constant (`[1]`). The heuristic you're using is to check if the `upper` and `step` element aren't present. But, we've already identified that the node is a slice using the match above.

Examples:
* `[1]` will be a `Constant` node
* `[1:]` will be a `Slice` node with `upper` and `step` as `None`

I'm just trying to figure out the motivation for using this heuristic to check if the node is `Slice` or not as I think we could remove it.

---

_@dhruvmanila reviewed on 2023-07-10 04:17_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:57 on 2023-07-10 04:17_

Oh, sorry I missed this in my previous review but for slice the suggestion will be different.

```
Replace with `[next(iter({}))]`
```

Similar change in the violation message.

---

_@evanrittenhouse reviewed on 2023-07-10 11:48_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:57 on 2023-07-10 11:48_

Oh, good point. Thanks!

---

_@evanrittenhouse reviewed on 2023-07-10 12:10_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/unnecessary_list_allocation_for_first_element.rs`:164 on 2023-07-10 12:10_

Ah, I see your point - yep, this is an unnecessary heuristic and a relic of old changes. Thanks for the detailed review

---

_Review requested from @dhruvmanila by @dhruvmanila on 2023-07-10 13:00_

---

_Comment by @charliermarsh on 2023-07-10 13:56_

Will get to this today.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:166 on 2023-07-10 15:40_

Can `&mut` be removed here?

---

_@charliermarsh reviewed on 2023-07-10 15:40_

---

_@charliermarsh reviewed on 2023-07-10 15:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:199 on 2023-07-10 15:40_

Nit: can use `elt.is_name_expr` here.

---

_@charliermarsh reviewed on 2023-07-10 15:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:205 on 2023-07-10 15:41_

Nit: can use `let [generator] = generators.as_slice() else { return None; }` here.

---

_@charliermarsh reviewed on 2023-07-10 15:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:215 on 2023-07-10 15:41_

I think this down to the end of the method is equivalent to just `get_name_id(&generator.iter)`, right?

---

_Label `rule` added by @charliermarsh on 2023-07-10 16:23_

---

_Merged by @charliermarsh on 2023-07-10 16:32_

---

_Closed by @charliermarsh on 2023-07-10 16:32_

---

_Branch deleted on 2023-07-10 16:37_

---

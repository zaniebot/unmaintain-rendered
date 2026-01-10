```yaml
number: 14319
title: "[`ruff`] Implement `redundant-bool-literal` (`RUF038`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: redundant-bool-literal
created_at: 2024-11-13T14:10:42Z
updated_at: 2024-11-16T22:02:07Z
url: https://github.com/astral-sh/ruff/pull/14319
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Implement `redundant-bool-literal` (`RUF038`)

---

_Pull request opened by @sbrugman on 2024-11-13 14:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements `redundant-bool-literal`

## Test Plan

<!-- How was it tested? -->

`cargo test`

The ecosystem results are all correct, but for `Airflow` the rule is not relevant due to the use of overloading (and is marked as unsafe correctly).


---

_Comment by @github-actions[bot] on 2024-11-13 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/76ce15a4c322bb8d5f49dd384e055b782118c985/airflow/models/dagrun.py#L1317'>airflow/models/dagrun.py:1317:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/apache/airflow/blob/76ce15a4c322bb8d5f49dd384e055b782118c985/airflow/models/dagrun.py#L1439'>airflow/models/dagrun.py:1439:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/f165bd57b7fd8334a9d65329e19f7090842007e7/pandas/core/dtypes/dtypes.py#L1271'>pandas/core/dtypes/dtypes.py:1271:15:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/ast.pyi#L1480'>stdlib/ast.pyi:1480:16:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/ast.pyi#L1481'>stdlib/ast.pyi:1481:35:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/ast.pyi#L1484'>stdlib/ast.pyi:1484:45:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stubs/pyxdg/xdg/Menu.pyi#L97'>stubs/pyxdg/xdg/Menu.pyi:97:11:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/aabdc9b0a142d8c277f2c462ad28893624ef9314/nox/command.py#L33'>nox/command.py:33:16:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF038 | 7 | 7 | 0 | 0 | 0 |
| RUF036 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@MichaReiser approved on 2024-11-14 07:39_

LGTM. Thank you and I especially like the detailed fix safety documentation. @AlexWaygood would you mind signing off the rule?

---

_Label `rule` added by @MichaReiser on 2024-11-14 07:39_

---

_Label `preview` added by @MichaReiser on 2024-11-14 07:39_

---

_Comment by @MichaReiser on 2024-11-14 07:42_

What do you think of `true-false-literal` as a rule name? Because the first thing I think of when reading redundant is that the rule is about `Literal[True, True]` when it isn't

---

_Comment by @sbrugman on 2024-11-14 07:48_

I've got no strong naming preference. I considered `true-false`, but then `Literal[True, False, True, False]` are also detected. The criterium is that the values in the literal are exhaustive for the type. 

---

_@AlexWaygood reviewed on 2024-11-14 13:33_

Hmm, how common is this antipattern in practice? I see a few ecosystem hits here, but not enough to make me think that this is something that a _lot_ of users are doing. Has this rule been requested by any of our userse?

For context: we've had lots of conversations over at flake8-pyi over whether we should implement more rules that detect redundant unions (https://github.com/PyCQA/flake8-pyi/issues/283, https://github.com/PyCQA/flake8-pyi/issues/414, etc.). Our conclusion every time has been that without very sophisticated multifile type inference, a generalised way of detecting redundant union elements is impossible, so we should only implement rules that detect common mistakes that we've actually encountered in real code.

---

_Comment by @sbrugman on 2024-11-14 14:19_

> Hmm, how common is this antipattern in practice? I see a few ecosystem hits here, but not enough to make me think that this is something that a lot of users are doing. Has this rule been requested by any of our users?

It's a fair concern, but I would argue to give it a try and see how it turns out for users.

I've searched before implementing this rule, and it's less rare than I expected (see list below). 
The rule is under preview, so we could always later decide to remove it if users do not pick it up/disable it.

For the nested unions and literals, having the rules in place can simplify the auto fixes of other rules, since we can reduce the problem to one that we've already solved.

We also should remind ourselves that the ecosystem results are published code, it could be that while developing/refactoring, such rules become more relevant. For instance saving users time when running `ruff` on save.

It's good to consider that is that if you expect typed Python to be more common with the amazing tooling that is coming available (type checkers, auto typing etc.), the number of users of these features also increase, and so will their need for learning new concepts such as `Literal`. I like to think of rules as a tool for users who are learning new concepts such as typing, and then realising when `Literal[True, False]` is equivalent to `bool` and when not could help in developing their mental model. A bit off topic, but this would be one of the cool outcomes of grouping rules related to the same language features together (#1774).

One final and exciting motivation is that these rules add to the diversity of test cases too, which can help testing red-knot!

Some more results:
- https://github.com/spyoungtech/ahk/blob/main/ahk/_sync/engine.py#L1300
- https://github.com/vocodedev/vocode-core/blob/main/vocode/streaming/agent/streaming_utils.py#L43
- https://github.com/strawberry-graphql/strawberry/blob/main/strawberry/types/mutation.py#L140
- https://github.com/langchain-ai/langchain/blob/master/libs/community/langchain_community/llms/openllm.py#L110
- https://github.com/Lightning-AI/pytorch-lightning/blob/master/src/lightning/pytorch/loggers/mlflow.py#L122
- https://github.com/nipy/heudiconv/blob/master/heudiconv/dicoms.py#L263


---

_Comment by @AlexWaygood on 2024-11-14 15:21_

> I like to think of rules as a tool for users who are learning new concepts such as typing, and then realising when `Literal[True, False]` is equivalent to `bool` and when not could help in developing their mental model. A bit off topic, but this would be one of the cool outcomes of grouping rules related to the same language features together (#1774).

Absolutely! I'm sure that this rule would help some people.

My concern is just that there's sort of an endless number of rules like this, regarding redundant union members, that you could plausibly add to Ruff. It's not clear where to draw the line, and they all add a certain amount of maintenance burden for us. When we adopt red-knot as the new backend, moreover, we should be able to consolidate them all into a single rule that is implemented with much less code and has much greater accuracy.

So I'm not against adding these rules when they address very common user issues or when we've been explicitly asked for a rule like this. If I were dead-set against it, I wouldn't have added rules like this to flake8-pyi in the first place! But I do think the bar needs to be ~reasonably high for this kind of rule.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:18 on 2024-11-14 16:22_

Start off with a short sentence that clearly states that this is a rule concerned with readability and style rather than correctness:

```suggestion
/// `Literal[True, False]` can be replaced with `bool` in type annotations,
/// which means the same thing but is more concise and more readable. 
///
/// This is because the `bool` type has exactly two constant instances:
/// `True` and `False`. Static type checkers such as [mypy] treat
/// `Literal[True, False]` as equivalent to `bool` in a type annotation.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:23 on 2024-11-14 16:23_

```suggestion
/// from typing import Literal
///
/// x: Literal[True, False]
/// y: Literal[True, False, "hello", "world"]
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:29 on 2024-11-14 16:23_

```suggestion
/// from typing import Literal
///
/// x: bool
/// y: Literal["hello", "world"] | bool
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:41 on 2024-11-14 16:24_

```suggestion
/// - The `Literal` slice might contain trailing-line comments which the fix would
///   remove.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:40 on 2024-11-14 16:25_

```suggestion
///   treat `bool` as equivalent when overloading boolean arguments with `Literal[True]`
///   and `Literal[False]`, e.g. see [#14764] and [#5421].
/// - `bool` is not strictly equivalent to `Literal[True, False]`, as `bool` is
///   a subclass of `int`, and this rule might not apply if the type annotations are used
///   in a numerical context.
```

---

_@AlexWaygood approved on 2024-11-14 16:26_

I chatted with @MichaReiser offline and he convinced me that this would be an especially helpful rule for Python beginners. I'm still a bit concerned that there'll be a lot of churn for users if we end up deprecating all these rules and replacing them with a single, generalised rule when we switch to using red-knot as the backend. But that's probably a long way off.

Just a few docs nitpicks, then I think we're good to go 👍

---

_@AlexWaygood reviewed on 2024-11-14 16:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/redundant_bool_literal.rs`:40 on 2024-11-14 16:29_

(arguably it's a type-checker bug if they don't understand that `<object of type Literal[True]> + <object of type Literal[True]> == <object of type Literal[2]>` in a numeric context. But hey.)

---

_@charliermarsh approved on 2024-11-16 18:29_

---

_Comment by @charliermarsh on 2024-11-16 18:31_

Applying @AlexWaygood comments, then merging.

---

_Review requested from @carljm by @MichaReiser on 2024-11-16 21:48_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-16 21:48_

---

_Merged by @charliermarsh on 2024-11-16 21:52_

---

_Closed by @charliermarsh on 2024-11-16 21:52_

---

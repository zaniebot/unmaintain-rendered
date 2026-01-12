```yaml
number: 14639
title: Fix bug where methods defined using lambdas were flagged by FURB118
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: alex/furb118
created_at: 2024-11-27T17:52:14Z
updated_at: 2024-11-28T12:59:57Z
url: https://github.com/astral-sh/ruff/pull/14639
synced_at: 2026-01-12T15:55:48Z
```

# Fix bug where methods defined using lambdas were flagged by FURB118

---

_@AlexWaygood_

## Summary

Fixes #13829.

Replacing an instance method definition with an `operator`-module function doesn't work, because instance methods need to be callable objects with `__get__` methods ("descriptors"), and while user-defined functions are descriptors, this is not true for the `operator`-module functions. We already avoid flagging user-defined methods that are defined using `def` statements, but we currently incorrectly flag user-defined functions that are defined by assigning `lambda`s to variables in class bodies. I.e., this is a perfectly valid definition of an instance method, and FURB118's suggestion to replace the lambda with `operator.eq` function would break it:

```py
class Spam:
    foo = lambda self, other: self == other
```

As a demonstration in the REPL:

```pycon
>>> class Spam:
...     foo = lambda self, other: self == other
...     
>>> Spam().foo(Spam())
False
>>> from operator import eq
>>> class Spam:
...     foo = eq
...     
>>> Spam().foo(Spam())
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    Spam().foo(Spam())
    ~~~~~~~~~~^^^^^^^^
TypeError: eq expected 2 arguments, got 1
```

This PR fixes things so that `lambda` assignments in class bodies are considered off-limits for the rule in the same way as `def` statements in class bodies. It also makes the fix-safety docs more expansive, so that they're clearer that there's a wider range of reasons why the rule might be unsafe than just the keyword-argument issue.

## Test Plan

I added a new fixture that confirms that all function definitions in class bodies are ignored by the rule.


---

_Label `bug` added by @AlexWaygood on 2024-11-27 17:52_

---

_Label `preview` added by @AlexWaygood on 2024-11-27 17:52_

---

_Comment by @github-actions[bot] on 2024-11-27 17:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/models/custom_profile_fields.py#L131'>zerver/models/custom_profile_fields.py:131:81:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-11-27 18:03_

Hmm, the `airflow` hits in the ecosystem check are false negatives. I think I can avoid them.

---

_@AlexWaygood reviewed on 2024-11-27 18:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1657 on 2024-11-27 18:23_

This needed to be moved from `deferred_lambdas.rs` to `expressions.rs` because otherwise the semantic model doesn't understand that the lambda it's analysing is ever inside a class scope. As far as I can tell, there's no particular advantage associated with deferring the analysis of this rule to after the semantic model has been fully built (what we currently do). The rule never analyses any bindings or uses of bindings

---

_Comment by @AlexWaygood on 2024-11-27 18:32_

https://github.com/astral-sh/ruff/pull/14639/commits/7e13565d1feb8af7a2b8c6fe0e1b8066195c0cc9 got rid of the new false negatives this PR initially introduced on `airflow`. The remaining ecosystem hit on `zerver` is also a false negative introduced by this PR, but I think it's acceptable personally. Flagging lambdas inside assignments in class scopes is too risky.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB118.py`:109 on 2024-11-28 06:52_

Could we add an example for the zerver case documenting that we're currently not detecting this pattern but probably should?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1657 on 2024-11-28 06:54_

Thanks for writing this comment. It saved me quiet some time guessing why it was moved.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:106 on 2024-11-28 06:58_

A comment explaining why we're testing for an assignment might be helpful. 

I wonder if we could avoid the false positive by testing if there's a parent expression. If that's the case, then the operator can't be the assignment's value?

---

_@MichaReiser approved on 2024-11-28 06:58_

Thank you

---

_@AlexWaygood reviewed on 2024-11-28 11:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:106 on 2024-11-28 11:07_

> I wonder if we could avoid the false positive by testing if there's a parent expression. If that's the case, then the operator can't be the assignment's value?

I don't think that's the case -- a lot of functions take an input function, do something with the function (monkey-patch an attribute onto the function, or store the function in a global registry somewhere, etc.), then return the same function. That's a very common pattern for decorators, for example:
- https://github.com/python/cpython/blob/3a77980002845c22e5b294ca47a12d62bf5baf53/Lib/typing.py#L2706-L2739
- https://github.com/python/cpython/blob/3a77980002845c22e5b294ca47a12d62bf5baf53/Lib/typing.py#L3738-L3770
- https://github.com/python/cpython/blob/3a77980002845c22e5b294ca47a12d62bf5baf53/Lib/typing.py#L2582-L2615

So all of these are still valid method definitions, and it would be a false positive if we flagged them with FURB118:

```py
from typing import final, override, no_type_check

class Foo:
    a = final(lambda self, other: self == other)
    b = override(lambda self, other: self == other)
    c = no_type_check(lambda self, other: self == other)
    d = final(override(no_type_check(lambda self, other: self == other)))
```

I'll add more tests involving constructs like these

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:106 on 2024-11-28 11:10_

One heuristic that might work would be if we still emitted diagnostics for lambdas in `key=` arguments, since lambdas are very often used as `key` functions and it's very likely that the lambda will be thrown away if it's passed as a `key` function. That would avoid the false negative on the `zerver` codebase.

Complex heuristics like that make the rule harder to understand for users, though, so there's a tradeoff there. I think I'd prefer to avoid it for now.

---

_@AlexWaygood reviewed on 2024-11-28 11:10_

---

_@MichaReiser reviewed on 2024-11-28 12:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:106 on 2024-11-28 12:05_

Oh, okay, that's indeed complicated. I'm fine keeping it as is.

---

_@AlexWaygood reviewed on 2024-11-28 12:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1657 on 2024-11-28 12:44_

it took _me_ a little while to figure out why the semantic model didn't think lambdas inside class scopes were inside class scopes 😅

---

_@AlexWaygood reviewed on 2024-11-28 12:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/refurb/FURB118.py`:109 on 2024-11-28 12:47_

done

---

_Merged by @AlexWaygood on 2024-11-28 12:58_

---

_Closed by @AlexWaygood on 2024-11-28 12:58_

---

_Branch deleted on 2024-11-28 12:58_

---

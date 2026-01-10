```yaml
number: 12762
title: "`RUF031`: Ignore unparenthesized tuples in subscripts when the subscript is obviously a type annotation or type alias"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: alex/annotation-tuples
created_at: 2024-08-08T18:40:37Z
updated_at: 2024-08-09T19:40:03Z
url: https://github.com/astral-sh/ruff/pull/12762
synced_at: 2026-01-10T21:47:02Z
```

# `RUF031`: Ignore unparenthesized tuples in subscripts when the subscript is obviously a type annotation or type alias

---

_Pull request opened by @AlexWaygood on 2024-08-08 18:40_

## Summary

Fixes #12758. Previously with the setting `lint.ruff.parenthesize-tuple-in-subscript = true`, RUF031 would have complained about annotations such as `dict[str, int]`, telling you that you should spell them as `dict[(str, int)]` instead. That feels somewhat silly -- although it is of course a subscription in exactly the same way as a dictionary key lookup, it's not really what the rule is trying to detect.

This PR fixes most of those cases. Unfortunately it doesn't fix cases like this, where it's ambiguous whether the variable is a type alias or not:

```py
import typing
X = typing.Callable[[str, int], bool]  # RUF031 here
```

Faced with this, however, users have a fairly simple workaround: make it explicit that it's a type alias, and the false positive will go away

```py
import typing
X: typing.TypeAlias = typing.Callable[[str, int], bool]
```

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `bug` added by @AlexWaygood on 2024-08-08 18:40_

---

_Label `rule` added by @AlexWaygood on 2024-08-08 18:40_

---

_Label `preview` added by @AlexWaygood on 2024-08-08 18:40_

---

_Comment by @github-actions[bot] on 2024-08-08 18:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/utils/test_train_utils.py#L117'>tests/utils/test_train_utils.py:117:67:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF031 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-08-08 18:58_

> [RasaHQ/rasa](https://github.com/RasaHQ/rasa) (+0 -1 violations, +0 -0 fixes)
> 
> ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview
> 
> - [tests/utils/test_train_utils.py:117:67:](https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/utils/test_train_utils.py#L117) RUF031 [*] Avoid parentheses for tuples in subscripts.

Oh, that's ugly. It sorta feels like a shame that we'll no longer be emitting a diagnostic there 😆

I suppose we *could* only ignore the rule for type annotations/aliases if `lint.ruff.parenthesize-tuple-in-subscript = true`, and continue to apply the rule everywhere if `lint.ruff.parenthesize-tuple-in-subscript = false`? But that feels conceptually harder to explain

---

_Comment by @dylwil3 on 2024-08-08 19:11_

Does it make sense to say in the docstring for the rule that we skip annotations, or is that too in the weeds?

---

_Comment by @AlexWaygood on 2024-08-08 19:13_

That's a good idea. I'll add some more docs tomorrow (it's late now where I am :-)

---

_@dylwil3 reviewed on 2024-08-08 19:26_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:68 on 2024-08-08 19:26_

Should we also do `semantic.in_simple_string_type_definition()` here?

---

_Comment by @dhruvmanila on 2024-08-09 05:56_

I think it might make sense to cover generics as well (https://github.com/astral-sh/ruff/issues/12773#issuecomment-2277195957) like:

```py
class Foo(Generic[T1, T2]):
	...
```

(It can be done as a follow-up.)

---

_Comment by @MichaReiser on 2024-08-09 06:27_

> such as dict[str, int], telling you that you should spell them as dict[(str, int)] instead. That feels somewhat silly -- although it is of course a subscription in exactly the same way as a dictionary key lookup, it's not really what the rule is trying to detect.

I find it hard to following this reasoning but it's probably just me not writing enough python. Isn't this as "silly" as doing the same in regular subscripts?

---

_Comment by @AlexWaygood on 2024-08-09 19:28_

> I find it hard to following this reasoning but it's probably just me not writing enough python. Isn't this as "silly" as doing the same in regular subscripts?

Type annotations just have their own specific conventions around them. And the subscription has a very different meaning in a typing context. You're not "looking up" a value at an index or key in a typing context in the same way; and I think the vast majority of people aren't even aware that they're creating implicit tuples in a type annotation context.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:68 on 2024-08-09 19:31_

It's not needed -- if the `SIMPLE_STRING_TYPE_DEFINITION` flag has been set on the semantic model, the `TYPE_DEFINITION` flag will also have been set:

https://github.com/astral-sh/ruff/blob/c4e651921ba1911a04102e2052c9fe56f185309d/crates/ruff_linter/src/checkers/ast/mod.rs#L2196-L2204

---

_@AlexWaygood reviewed on 2024-08-09 19:31_

---

_Merged by @AlexWaygood on 2024-08-09 19:31_

---

_Closed by @AlexWaygood on 2024-08-09 19:31_

---

_Branch deleted on 2024-08-09 19:31_

---

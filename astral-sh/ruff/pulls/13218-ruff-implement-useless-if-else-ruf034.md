```yaml
number: 13218
title: "[ruff] implement useless if-else (RUF034)"
type: pull_request
state: merged
author: vieira127
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: redundant-ternary-rule
created_at: 2024-09-02T21:16:10Z
updated_at: 2024-09-04T06:22:17Z
url: https://github.com/astral-sh/ruff/pull/13218
synced_at: 2026-01-12T15:55:43Z
```

# [ruff] implement useless if-else (RUF034)

---

_@vieira127_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Adds a new rule, RUF102, to check for redundant ternary operators. Message shows when both the true and false branches return the same value, as discussed in #12516

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
`cargo test`

---

_Comment by @github-actions[bot] on 2024-09-02 21:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/superset/utils/decorators.py#L158'>superset/utils/decorators.py:158:9:</a> RUF034 Useless if-else condition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/2244402942dbd30bdf367ceae49937c179e42bcb/pandas/tests/io/formats/style/test_style.py#L923'>pandas/tests/io/formats/style/test_style.py:923:26:</a> RUF034 Useless if-else condition
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF034 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @RubenVanEldik on 2024-09-02 21:55_

Hi Lucas,

I am new to Ruff an am just reading PRs to get a better understanding of how everything works. I was wondering, how did you decide to name it `RUF102`. Is there any logic behind the `RUF0XX`, `RUF1XX` sets?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:966 on 2024-09-03 08:04_

The `1XX` ruff rules are reserved for noqa specific rules. Let's change the code to 034.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 08:08_

I quickly checked other "ternary" specific rules and all of them use the term `if-expr` instead. We also use the term `Useless` when referring to *useless* code.

How about: `useless-if-expr`?

---

_@MichaReiser reviewed on 2024-09-03 08:15_

This overall looks great to me but we should change the rule code and its name.

I have some concerns with the scope of the rule. I know it has previously been accepted but I only just now looked closer into it. 

* There's some overlap with `SIM108` that simplifies if expressions
* The scope of the rule is very narrow. Should it also apply to if-statements?


@AlexWaygood what are your thoughts on the rule?

It would be great if the rule also provided a fix that removes the if expression with the true or false body. 

---

_Label `rule` added by @MichaReiser on 2024-09-03 08:15_

---

_Label `preview` added by @MichaReiser on 2024-09-03 08:15_

---

_Comment by @MichaReiser on 2024-09-03 08:16_

The ecosystem check shows an interesting false positive:

https://github.com/apache/superset/blob/548d543efe81ecd6f0a6657550230b765ab4d955/superset/utils/decorators.py#L158

---

_@vieira127 reviewed on 2024-09-03 10:28_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/codes.rs`:966 on 2024-09-03 10:28_

@RubenVanEldik I think this answers it 😄. This is also my first time contributing here.

---

_Comment by @AlexWaygood on 2024-09-03 10:36_

> The ecosystem check shows an interesting false positive:
> 
> [apache/superset@`548d543`/superset/utils/decorators.py#L158](https://github.com/apache/superset/blob/548d543efe81ecd6f0a6657550230b765ab4d955/superset/utils/decorators.py#L158)

@MichaReiser _is_ that a false positive? The snippet in question is:

```py
sorted_args = tuple(
    x if hasattr(x, "__repr__") else x for x in [*args, *sorted(kwargs.items())]
)
```

It seems like each element in the iterable there will be collected whether or not it has a `__repr__` method, so I think it can just be simplified to

```py
sorted_args = tuple(x for x in [*args, *sorted(kwargs.items())])
```

or even just

```py
sorted_args = (*args, *sorted(kwargs.items()))
```

What they probably _wanted_ to do was

```py
sorted_args = tuple(
    repr(x) if hasattr(x, "__repr__") else x for x in [*args, *sorted(kwargs.items())]
)
```

But even that doesn't make much sense, because all objects in Python have a `__repr__` attribute:

```pycon
>>> hasattr(object, "__repr__")
True
```

---

_@AlexWaygood reviewed on 2024-09-03 10:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 10:38_

`useless-if-expr` sounds good to me. I agree that that's more approachable language than `ternary`

---

_Comment by @AlexWaygood on 2024-09-03 10:42_

> There's some overlap with `SIM108` that simplifies if expressions

I support this being a separate rule to `SIM108`. `SIM108` is quite opinionated IMO: I think it's questionable whether rewriting an `if/else` block as a ternary expression is always easier to read in Python, or always more idiomatic. This rule, on the other hand, seems like it's pointing out something that you'd almost always want to avoid. A useless ternary expression is almost certainly a straight-up mistake that you didn't mean to write 😄

> The scope of the rule is very narrow. Should it also apply to if-statements?

Also applying it to if/else statements sounds reasonable to me.

---

_Comment by @vieira127 on 2024-09-03 11:28_

> It would be great if the rule also provided a fix that removes the if expression with the true or false body.

I don't think a fix would fit this rule because users probably end up in this situation when they mistype one of the ternary ends. Instead of just removing the ternary, they probably need to correct one of the sides.  

---

_@vieira127 reviewed on 2024-09-03 16:18_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 16:18_

Sounds good, but if we also cover if blocks maybe we will need a different name. For example: `useless-if-logic`

---

_@AlexWaygood reviewed on 2024-09-03 16:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 16:20_

hmm, good point. Maybe `useless-if-else`? Or `useless-if-condition`?

---

_Renamed from "[ruff] implement redundant ternary rule (RUF102)" to "[ruff] implement redundant ternary rule (RUF034)" by @AlexWaygood on 2024-09-03 16:21_

---

_Comment by @vieira127 on 2024-09-03 16:23_

> * The scope of the rule is very narrow. Should it also apply to if-statements?

I wonder if rule SIM114 address that

---

_@vieira127 reviewed on 2024-09-03 16:33_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 16:33_

I like`useless-if-else` 

---

_@MichaReiser reviewed on 2024-09-03 16:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1407 on 2024-09-03 16:33_

I like `useless-if-else`. It provides room for expanding the rule in the future.

---

_Comment by @MichaReiser on 2024-09-03 16:35_

`SIM114` only seems to flag `if-elif` but not `if-else`. https://play.ruff.rs/ae79ec83-f045-4262-bcfb-dc8a08a19460

---

_@AlexWaygood approved on 2024-09-03 19:04_

LGTM. I think we could merge this now and expand it to `if`/`else` statements as a followup. @MichaReiser what do you think?

---

_Renamed from "[ruff] implement redundant ternary rule (RUF034)" to "[ruff] implement useless if-else (RUF034)" by @MichaReiser on 2024-09-04 06:22_

---

_Merged by @MichaReiser on 2024-09-04 06:22_

---

_Closed by @MichaReiser on 2024-09-04 06:22_

---

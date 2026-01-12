```yaml
number: 14323
title: "[`ruff`] Implement `unnecessary-nested-literal` (`RUF041`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: unnecessary-nested-literal
created_at: 2024-11-13T16:20:51Z
updated_at: 2024-11-27T10:03:34Z
url: https://github.com/astral-sh/ruff/pull/14323
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Implement `unnecessary-nested-literal` (`RUF041`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implementing `unnecessary-nested-literal`.

This rule could help simplify other rules' fixes by handling the flattening of `Literal`s here. 

See also https://github.com/astral-sh/ruff/pull/14270/files#r1837810594 (unions in a follow-up PR)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test`

The ecosystem results are correct.

Some of the nesting emits multiple violations.
I've got a fix for this, but that depends on https://github.com/astral-sh/ruff/pull/14280. 
We can go on with merging this PR after review regardless (the violations are not wrong).

---

_Comment by @github-actions[bot] on 2024-11-13 16:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/f2594eea57de985a391b9a97e5879b3f283ead1d/tests/test_utils.py#L730'>tests/test_utils.py:730:18:</a> RUF041 [*] Unnecessary nested `Literal`
+ <a href='https://github.com/DisnakeDev/disnake/blob/f2594eea57de985a391b9a97e5879b3f283ead1d/tests/test_utils.py#L758'>tests/test_utils.py:758:10:</a> RUF041 [*] Unnecessary nested `Literal`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/4ce28d2539ab710f84d485430061345b4545a5f2/stdlib/xml/dom/pulldom.pyi#L23'>stdlib/xml/dom/pulldom.pyi:23:131:</a> E501 Line too long (153 > 130)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF041 | 2 | 2 | 0 | 0 | 0 |
| E501 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-11-14 07:40_

---

_Label `preview` added by @MichaReiser on 2024-11-14 07:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:81 on 2024-11-14 07:46_

You could consider using `AnyNodeRef::ptr_eq` here because all you want to test is if the expressions are the same (`Expr::eq` does a deep comparison by default)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:95 on 2024-11-14 07:48_

Can you expand on why we have to check inside the rule whether it is a nested literal, considering that the rule is only called when `semantic.in_nested_literal` is `true`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:98 on 2024-11-14 07:48_

Nit
```suggestion
    let mut diagnostic = Diagnostic::new(UnnecessaryNestedLiteral, literal_expr.range());
```

---

_@MichaReiser reviewed on 2024-11-14 07:49_

@AlexWaygood would you mind doing a quick glance at the rule definition? I already reviewed the code

---

_@sbrugman reviewed on 2024-11-14 07:58_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:95 on 2024-11-14 07:58_

We only consider the top-level union, so when `semantic.in_nested_literal` is `false`. I've considered to run this run only when `semantic.in_nested_literal` is `true`, but then the top-level union is unavailable for the autofix.

---

_@MichaReiser reviewed on 2024-11-14 08:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:95 on 2024-11-14 08:00_

Oh, we actually only run the rule when not in a nested literal... 

---

_@MichaReiser reviewed on 2024-11-14 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:76 on 2024-11-14 08:02_

It might also be worth to use a `SmallVec` here with a size of 1 to avoid allocating if this is a not-nested literal.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:12 on 2024-11-14 13:39_

The direct answer to the question of "Why is this bad?" is "it's less readable than the alternative". I think something to that effect should be the first sentence of this section, as it makes it clear that this rule is about readability and style rather than correctness.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:52 on 2024-11-14 13:40_

```suggestion
/// The fix for this rule is marked as unsafe when the `Literal` slice is split
/// across multiple lines and some of the lines have trailing comments.
```

---

_@AlexWaygood reviewed on 2024-11-14 13:42_

Similar to https://github.com/astral-sh/ruff/pull/14319#pullrequestreview-2436098516, I feel like I'm not sure how much this antipattern really comes up in practice. But, I can see the value if this is a pattern that could be introduced by the fix for other rules we implement!

---

_@sbrugman reviewed on 2024-11-14 15:34_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:76 on 2024-11-14 15:34_

`nodes` will be populated with all entries in `Literal`, so even for not-nested literals it can exceed 1. Shall I do a first pass to check if the `Literal` is nested, followed by another to collect the nodes? This will reduce the allocation on the common path.

---

_Comment by @MichaReiser on 2024-11-25 09:44_

Is there any open feedback that needs addressing? I'm otherwise happy to merge this rule.

---

_Comment by @AlexWaygood on 2024-11-25 11:17_

My feedback has been addressed, but it looks like there's quite a few merge conflicts here. The conversations in https://github.com/astral-sh/ruff/pull/14323#discussion_r1841713308 and https://github.com/astral-sh/ruff/pull/14323#discussion_r1841729372 are also not marked as "resolved", and I think you're better placed to judge whether they should be or not ;)

---

_@MichaReiser reviewed on 2024-11-25 12:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_nested_literal.rs`:76 on 2024-11-25 12:04_

I think checking first and then collecting could be good for performance

---

_Renamed from "[`ruff`] Implement `unnecessary-nested-literal` (`RUF039`)" to "[`ruff`] Implement `unnecessary-nested-literal` (`RUF041`)" by @MichaReiser on 2024-11-27 09:18_

---

_Merged by @MichaReiser on 2024-11-27 10:01_

---

_Closed by @MichaReiser on 2024-11-27 10:01_

---

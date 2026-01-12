```yaml
number: 15779
title: "[`refurb`] Avoid `None | None` as well as better detection and fix (`FURB168`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: FURB168
created_at: 2025-01-28T02:02:33Z
updated_at: 2025-01-31T16:20:07Z
url: https://github.com/astral-sh/ruff/pull/15779
synced_at: 2026-01-12T15:55:52Z
```

# [`refurb`] Avoid `None | None` as well as better detection and fix (`FURB168`)

---

_@InSyncWithFoo_

## Summary

Resolves #15776.

The rule has been rewritten to address the problems discussed at the aforementioned issue. Additionally, `Union[]` is now detected and the fix marked as unsafe if there are any comments in the range.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-28 02:07_

[`refurb::helpers::generate_none_identity_comparison()`](https://github.com/astral-sh/ruff/blob/9c938442e5148137a3d4fcf4a8e7d33f7e19cdf6/crates/ruff_linter/src/rules/refurb/helpers.rs#L43) and other rules using it need to be rewritten as well. I'll take care of those in a follow-up PR.

---

_Comment by @github-actions[bot] on 2025-01-28 02:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9f737a77877e46c4006da73d26b38c9046e26f6d/tests/core/test_configuration.py#L906'>tests/core/test_configuration.py:906:16:</a> FURB168 [*] Prefer `is` operator over `isinstance` to check if an object is `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB168 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9f737a77877e46c4006da73d26b38c9046e26f6d/tests/core/test_configuration.py#L906'>tests/core/test_configuration.py:906:16:</a> FURB168 [*] Prefer `is` operator over `isinstance` to check if an object is `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB168 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:100 on 2025-01-28 10:27_

I think we should probably also check that `arguments.keywords.is_empty()`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:121 on 2025-01-28 10:33_

This doesn't need to be `&mut`, since you don't use any methods on `Checker` in this function that require a mutable reference

```suggestion
fn replace_with_identity_check(expr: &Expr, range: TextRange, checker: &Checker) -> Fix {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:157 on 2025-01-28 10:33_

Using the generator seems overkill for this. Couldn't we just do it like this?

```suggestion
    let semantic = checker.semantic();
    let lhs_source = &checker.source()[expr.range()];

    let new_content = if semantic.current_expression_parent().is_some() {
        format!("({lhs_source} is None)")
    } else {
        format!("{lhs_source} is None")
    };
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:119 on 2025-01-28 10:39_

I think the implication of https://github.com/astral-sh/ruff/issues/15776 is really that we should _never_ emit this diagnostic for `isinstance()` used with PEP-604 unions including `None`. We don't want to emit a lint on this -- this rule is concerned with style, but there's a correctness error here:

```py
isinstance(x, None | None)`
```

But we also don't want to emit a lint on this, I don't think:

```py
isinstance(x, str | None)`
```

Because it isn't really a _simplification_ to rewrite that as

```py
isinstance(x, str) or x is None
```

But having said that, maybe we _should_ be emitting a lint on these (and we don't currently). They only work on Python 3.10+:

```py
from typing import Union

isinstance(x, Union[None])
isinstance(x, Union[None, None])
```

It's unlikely that anybody would write those kinds of things deliberately, but I could easily see them being produced by codemod tools or code-generation tools.

---

_@AlexWaygood reviewed on 2025-01-28 10:39_

Thank you!

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-28 10:57_

---

_Label `bug` added by @AlexWaygood on 2025-01-28 12:35_

---

_@InSyncWithFoo reviewed on 2025-01-28 15:24_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:157 on 2025-01-28 15:24_

Probably not. I don't want to deal with `a and b is None`. The generator has just got a boost, so I feel like we should rely on it more.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:119 on 2025-01-28 15:24_

Actually, we <em>do</em> want to report this:

```python
isinstance(x, type(None) | type(None))
```

`str | None` is not reported and has never been. `Union` is now also detected.

---

_@InSyncWithFoo reviewed on 2025-01-28 15:24_

---

_@AlexWaygood reviewed on 2025-01-29 11:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:157 on 2025-01-29 11:14_

> I don't want to deal with `a and b is None`.

Oh, I think I see what you mean. Could you possibly add a test that would fail with my suggestion, in that case? Otherwise I worry that someone (like me) might come along later and try to "simplify" this autofix :-)

---

_@AlexWaygood reviewed on 2025-01-29 19:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:163 on 2025-01-29 19:28_

Please could you add a `## Fix safety` section to the docs stating why the fix is sometimes marked as unsafe?

---

_@AlexWaygood approved on 2025-01-31 11:34_

thanks!

---

_Merged by @AlexWaygood on 2025-01-31 11:34_

---

_Closed by @AlexWaygood on 2025-01-31 11:34_

---

_Branch deleted on 2025-01-31 16:20_

---

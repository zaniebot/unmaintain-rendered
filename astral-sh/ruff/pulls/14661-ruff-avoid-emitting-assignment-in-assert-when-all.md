```yaml
number: 14661
title: "[`ruff`] Avoid emitting `assignment-in-assert` when all references to the assigned variable are themselves inside `assert`s (`RUF018`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: alex/ruf018
created_at: 2024-11-28T19:08:24Z
updated_at: 2024-11-29T13:37:01Z
url: https://github.com/astral-sh/ruff/pull/14661
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] Avoid emitting `assignment-in-assert` when all references to the assigned variable are themselves inside `assert`s (`RUF018`)

---

_@AlexWaygood_

## Summary

Fixes #14658.

The stated rationale for RUF018 is that it's unsafe to assign a variable using an assignment expression inside an `assert` statement, because if you run Python in optimised mode the assertion might be stripped out of the program at compilation time, meaning the variable is never actually defined. This concern is irrelevant, however, if the variable is only ever read from inside other `assert` statements: either all `assert` statements will be stripped from the Python program, or none will be. This PR therefore moves the rule to `bindings.rs` and modifies the rule so that it iterates through all references to the binding introduced by the assignment expression to see whether those references take place inside `assert` statements or not.

In order to determine whether a `Binding` or `ResolvedReference` took place inside an `assert` statement, it's necessary to add `SemanticModelFlags::ASSERT_STATEMENT` and `BindingFlags::IN_ASSERT_STATEMENT` to Ruff's semantic model.

## Test Plan

New fixtures added. `cargo test -p ruff_linter --lib`.


---

_Label `bug` added by @AlexWaygood on 2024-11-28 19:08_

---

_Comment by @github-actions[bot] on 2024-11-28 19:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+13 -3 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b882246702ccb040243db803c08d2097d67283a5/providers/tests/google/cloud/transfers/test_sql_to_gcs.py#L487'>providers/tests/google/cloud/transfers/test_sql_to_gcs.py:487:68:</a> RUF018 Avoid assignment expressions in `assert` statements
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/93ba8e16c3f1218c564e949ddf3de93a19125b6c/tests/unit_tests/db_engine_specs/utils.py#L44'>tests/unit_tests/db_engine_specs/utils.py:44:13:</a> RUF018 Avoid assignment expressions in `assert` statements
- <a href='https://github.com/apache/superset/blob/93ba8e16c3f1218c564e949ddf3de93a19125b6c/tests/unit_tests/db_engine_specs/utils.py#L60'>tests/unit_tests/db_engine_specs/utils.py:60:13:</a> RUF018 Avoid assignment expressions in `assert` statements
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L266'>ibis/common/tests/test_patterns.py:266:62:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L267'>ibis/common/tests/test_patterns.py:267:58:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L269'>ibis/common/tests/test_patterns.py:269:62:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L801'>ibis/common/tests/test_patterns.py:801:78:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L803'>ibis/common/tests/test_patterns.py:803:70:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L806'>ibis/common/tests/test_patterns.py:806:70:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L918'>ibis/common/tests/test_patterns.py:918:72:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L922'>ibis/common/tests/test_patterns.py:922:64:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L924'>ibis/common/tests/test_patterns.py:924:68:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L935'>ibis/common/tests/test_patterns.py:935:41:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L951'>ibis/common/tests/test_patterns.py:951:86:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L968'>ibis/common/tests/test_patterns.py:968:71:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L973'>ibis/common/tests/test_patterns.py:973:71:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 13 | 13 | 0 | 0 | 0 |
| RUF018 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -3 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b882246702ccb040243db803c08d2097d67283a5/providers/tests/google/cloud/transfers/test_sql_to_gcs.py#L487'>providers/tests/google/cloud/transfers/test_sql_to_gcs.py:487:68:</a> RUF018 Avoid assignment expressions in `assert` statements
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/93ba8e16c3f1218c564e949ddf3de93a19125b6c/tests/unit_tests/db_engine_specs/utils.py#L44'>tests/unit_tests/db_engine_specs/utils.py:44:13:</a> RUF018 Avoid assignment expressions in `assert` statements
- <a href='https://github.com/apache/superset/blob/93ba8e16c3f1218c564e949ddf3de93a19125b6c/tests/unit_tests/db_engine_specs/utils.py#L60'>tests/unit_tests/db_engine_specs/utils.py:60:13:</a> RUF018 Avoid assignment expressions in `assert` statements
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L266'>ibis/common/tests/test_patterns.py:266:62:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L267'>ibis/common/tests/test_patterns.py:267:58:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L269'>ibis/common/tests/test_patterns.py:269:62:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L801'>ibis/common/tests/test_patterns.py:801:78:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L803'>ibis/common/tests/test_patterns.py:803:70:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L806'>ibis/common/tests/test_patterns.py:806:70:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L918'>ibis/common/tests/test_patterns.py:918:72:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L922'>ibis/common/tests/test_patterns.py:922:64:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L924'>ibis/common/tests/test_patterns.py:924:68:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L935'>ibis/common/tests/test_patterns.py:935:41:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L951'>ibis/common/tests/test_patterns.py:951:86:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L968'>ibis/common/tests/test_patterns.py:968:71:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
+ <a href='https://github.com/ibis-project/ibis/blob/1a8300a2cc3daabbd8caf3bfcc6d5b7d3cc6a8af/ibis/common/tests/test_patterns.py#L973'>ibis/common/tests/test_patterns.py:973:71:</a> RUF100 Unused `noqa` directive (unused: `RUF018`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 13 | 13 | 0 | 0 | 0 |
| RUF018 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-11-28 19:17_

The ecosystem hits all look good to me

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/bindings.rs`:95 on 2024-11-29 05:11_

Should we just update the `diagnostics` vector inside the rule logic to avoid doing this here? We usually do that in other rules.

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/binding.rs`:285 on 2024-11-29 05:14_

Should this method be just `expression` instead? Considering `assert (x := 1)`, for the binding `x` this would return the named expression, right? If so, shouldn't that be the "expression in which the binding was defined" (similar to the `statement` method)?

---

_@dhruvmanila reviewed on 2024-11-29 05:16_

---

_@AlexWaygood reviewed on 2024-11-29 13:24_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/binding.rs`:285 on 2024-11-29 13:24_

Yeah, that makes sense. I named it like this because the implementation is more similar to `Binding::parent_statement()` than to `Binding::statement()`. That's because `self.source.and_then(|expression_id| semantic.expression(expression_id))` will give you the `ast::Expr::Name()` that represents the symbol `x` rather than the `ast::Expr::Named()` that represents the `x := 1` walrus. But I don't think you ever really want the `ast::Expr::Name` node (you can get the range directly from the `Binding`), so your naming is likely more intuitive for users of the API.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/bindings.rs`:95 on 2024-11-29 13:26_

We do that for most rules in `expressions.rs` and `statement.rs`, but we don't do that for any rules in `bindings.rs` for some reason. I'm happy to look separately into whether we should change the structure of these rules to make them more consistent with the majority of other AST-based rules, but for now I think local consistency within this file probably wins.

---

_@AlexWaygood reviewed on 2024-11-29 13:26_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-11-29 13:29_

---

_@dhruvmanila reviewed on 2024-11-29 13:34_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/bindings.rs`:95 on 2024-11-29 13:34_

Oh, that's fine then, no need to spend any more time here.

---

_@dhruvmanila approved on 2024-11-29 13:36_

Thanks!

---

_Merged by @AlexWaygood on 2024-11-29 13:37_

---

_Closed by @AlexWaygood on 2024-11-29 13:37_

---

_Branch deleted on 2024-11-29 13:37_

---

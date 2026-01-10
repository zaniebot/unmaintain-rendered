```yaml
number: 16155
title: "[refurb] Check for subclasses includes subscript expressions (FURB189)"
type: pull_request
state: merged
author: vladNed
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: bugfix/furb-189-rule-not-applied-when-type-args-provided
created_at: 2025-02-14T08:13:26Z
updated_at: 2025-02-15T03:38:28Z
url: https://github.com/astral-sh/ruff/pull/16155
synced_at: 2026-01-10T19:57:22Z
```

# [refurb] Check for subclasses includes subscript expressions (FURB189)

---

_Pull request opened by @vladNed on 2025-02-14 08:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Added checks for subscript expressions on builtin classes as in FURB189. 
The object is changed to use the collections objects and the types from the subscript are kept.

Resolves #16130 

> Note: Added some comments in the code explaining why
## Test Plan

<!-- How was it tested? -->
- Added a subscript dict and list class to the test file.
- Tested locally to check that the symbols are changed and the types are kept.
- No modifications changed on optional `str` values.


---

_Comment by @github-actions[bot] on 2025-02-14 08:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8f63b828ace09b1095229f22b0c6d1f0f85ea81a/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L136'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:136:20:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/ffe9244458d03995b760638e9bb5b347c0cc68d6/superset/db_engine_specs/trino.py#L407'>superset/db_engine_specs/trino.py:407:30:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L73'>src/bokeh/util/compiler.py:73:16:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L12'>src/typings/bs4.pyi:12:17:</a> FURB189 Subclassing `list` can be error prone, use `collections.UserList` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/3dffee3d0b037db1b380370e58aa08898baaf761/libs/core/langchain_core/runnables/utils.py#L465'>libs/core/langchain_core/runnables/utils.py:465:19:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/10bae9577c0f898f7ae9b2d540336058bda53837/tests/units/utils/test_types.py#L50'>tests/units/utils/test_types.py:50:18:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
+ <a href='https://github.com/reflex-dev/reflex/blob/10bae9577c0f898f7ae9b2d540336058bda53837/tests/units/vars/test_base.py#L9'>tests/units/vars/test_base.py:9:18:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/5eb5afbcfb42661a43a90c199c0e82a78d1f97fa/src/trio/_core/_instrumentation.py#L24'>src/trio/_core/_instrumentation.py:24:19:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB189 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2025-02-14 15:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:82 on 2025-02-14 15:57_

I think we could use `map_subscript` here, similar to e.g. 

https://github.com/astral-sh/ruff/blob/46fe17767d3d5cf7b1f7729b024fca7821d19264/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_contextvar_default.rs#L98-L100

---

_@MichaReiser approved on 2025-02-14 15:58_

Nice. I've a small nit comment but that otherwise looks good to go.

---

_Label `rule` added by @MichaReiser on 2025-02-14 15:59_

---

_Label `preview` added by @MichaReiser on 2025-02-14 15:59_

---

_Review comment by @vladNed on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:82 on 2025-02-14 18:08_

didn't know about the `map_subscript` function. Just modified the logic to use it, seems a lot more clean.

---

_@vladNed reviewed on 2025-02-14 18:08_

---

_Review requested from @MichaReiser by @vladNed on 2025-02-14 18:14_

---

_Merged by @MichaReiser on 2025-02-14 19:21_

---

_Closed by @MichaReiser on 2025-02-14 19:21_

---

_Branch deleted on 2025-02-15 03:38_

---

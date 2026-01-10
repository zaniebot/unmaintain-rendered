```yaml
number: 22009
title: "[`flake8-use-pathlib`] Make fixes unsafe when types change in compound statements (`PTH104`, `PTH105`, `PTH109`, `PTH115`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: issue-21483
created_at: 2025-12-16T17:13:15Z
updated_at: 2025-12-17T17:33:13Z
url: https://github.com/astral-sh/ruff/pull/22009
synced_at: 2026-01-10T16:42:11Z
```

# [`flake8-use-pathlib`] Make fixes unsafe when types change in compound statements (`PTH104`, `PTH105`, `PTH109`, `PTH115`)

---

_Pull request opened by @chirizxc on 2025-12-16 17:13_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/21794

## Test Plan

`cargo nextest run flake8_use_pathlib`


---

_Renamed from "[`flake8-use-pathlib`]" to "[`flake8-use-pathlib`] Fix `PTH's` unsafe handling when return types change in compound statements" by @chirizxc on 2025-12-16 17:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 17:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -26 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -16 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/minor_release_command.py#L180'>dev/breeze/src/airflow_breeze/commands/minor_release_command.py:180:17:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/minor_release_command.py#L180'>dev/breeze/src/airflow_breeze/commands/minor_release_command.py:180:17:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L719'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:719:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L719'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:719:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L123'>dev/breeze/src/airflow_breeze/commands/release_command.py:123:23:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L123'>dev/breeze/src/airflow_breeze/commands/release_command.py:123:23:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L156'>dev/breeze/src/airflow_breeze/commands/release_command.py:156:23:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L156'>dev/breeze/src/airflow_breeze/commands/release_command.py:156:23:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L177'>dev/breeze/src/airflow_breeze/commands/release_command.py:177:19:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L177'>dev/breeze/src/airflow_breeze/commands/release_command.py:177:19:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L387'>dev/breeze/src/airflow_breeze/commands/release_command.py:387:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L387'>dev/breeze/src/airflow_breeze/commands/release_command.py:387:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L403'>dev/breeze/src/airflow_breeze/commands/release_command.py:403:19:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/commands/release_command.py#L403'>dev/breeze/src/airflow_breeze/commands/release_command.py:403:19:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/utils/reproducible.py#L61'>dev/breeze/src/airflow_breeze/utils/reproducible.py:61:21:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/e659539c002af35ff7b72ebb0eb0678c041dc808/dev/breeze/src/airflow_breeze/utils/reproducible.py#L61'>dev/breeze/src/airflow_breeze/utils/reproducible.py:61:21:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:16:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:16:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L246'>src/bokeh/util/compiler.py:246:20:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L246'>src/bokeh/util/compiler.py:246:20:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L160'>tests/support/util/filesystem.py:160:11:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L160'>tests/support/util/filesystem.py:160:11:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L154'>tests/unit/bokeh/application/handlers/test_code_runner.py:154:19:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L154'>tests/unit/bokeh/application/handlers/test_code_runner.py:154:19:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/snakemake/serialize.py#L152'>src/latch_cli/snakemake/serialize.py:152:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/snakemake/serialize.py#L152'>src/latch_cli/snakemake/serialize.py:152:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH109 | 26 | 0 | 0 | 0 | 26 |

</p>
</details>





---

_Label `fixes` added by @ntBre on 2025-12-17 15:18_

---

_Label `preview` added by @ntBre on 2025-12-17 15:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:96 on 2025-12-17 15:36_

I think I would prefer to keep this check only in the rules that are affected. This helper is used in many additional PTH rules. Should all of those have unsafe fixes, or only the four in the issue?

To me, only the comment check is general enough to share here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:225 on 2025-12-17 15:46_

I think this is too narrow a check. Even a simple assignment statement seems like it should cause an unsafe fix because we can't (or at least don't) verify what the user does with the variable:

```py
x = os.rename("a", "b")
```

I think what we actually want to check is that the [`current_statement_parent`](https://github.com/astral-sh/ruff/blob/1134e85972afb77f410eed90c4a86f18ab568b2f/crates/ruff_python_semantic/src/model.rs#L1261) is  a `Stmt::Expr`:


```suggestion
    checker.semantic().current_statement.is_expr_stmt()
```

Note that this negates the condition, so I guess a better name for the function would be `is_top_level_statement`, and the call sites will need a `!`.

Applying this locally caused tests for a few additional rules to fail, which supports my concern in the other comment.

---

_@ntBre requested changes on 2025-12-17 15:47_

Thanks for working on this! But I think we need to be a bit more careful here.

---

_Review requested from @ntBre by @chirizxc on 2025-12-17 16:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:218 on 2025-12-17 16:52_

I think these are only called in the same places now, what do you think about combining them into one function? I think that was the original intent actually, but we overlooked the statement case in the previous PR. Maybe a name like `is_top_level_expression_in_statement` would work well.

---

_@ntBre reviewed on 2025-12-17 16:53_

Thanks for the quick fix! I think the logic here is good to go, I just had one small suggestion about combining the two helper functions.

---

_Comment by @chirizxc on 2025-12-17 17:17_

> Thanks for the quick fix! I think the logic here is good to go, I just had one small suggestion about combining the two helper functions.

Yes, I think it makes sense to combine them.

---

_@ntBre approved on 2025-12-17 17:29_

Thank you!

---

_Renamed from "[`flake8-use-pathlib`] Fix `PTH's` unsafe handling when return types change in compound statements" to "[`flake8-use-pathlib`] Make fixes unsafe when types change in compound statements (`PTH104`, `PTH105`, `PTH109`, `PTH115`)" by @ntBre on 2025-12-17 17:31_

---

_Merged by @ntBre on 2025-12-17 17:31_

---

_Closed by @ntBre on 2025-12-17 17:31_

---

_Branch deleted on 2025-12-17 17:33_

---

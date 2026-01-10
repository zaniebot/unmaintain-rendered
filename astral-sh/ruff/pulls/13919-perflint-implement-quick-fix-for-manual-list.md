```yaml
number: 13919
title: "[`perflint`] implement quick-fix for `manual-list-comprehension` (`PERF401`)"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: perf401
created_at: 2024-10-24T23:29:38Z
updated_at: 2024-11-11T11:17:02Z
url: https://github.com/astral-sh/ruff/pull/13919
synced_at: 2026-01-10T20:50:57Z
```

# [`perflint`] implement quick-fix for `manual-list-comprehension` (`PERF401`)

---

_Pull request opened by @w0nder1ng on 2024-10-24 23:29_

## Summary

I implemented a fix to rewrite for-loops where PERF401 applies into `list.extend`s. Since the target is guaranteed to be a list, and the lint (seemingly) guarantees that the iterator doesn't reference itself, this should be valid for all for-loops where the lint is applicable. It would be nice to implement this to rewrite the entire for-loop/variable assignment into a single list comprehensions, but I thought of several cases where this would affect how the code works, so this would need more consideration to write.

## Test Plan

I tried this on local files and it seemed to work, but I couldn't find where tests for `ruff check --fix` lints go. I'd be happy to add tests if someone points me in the right direction.


---

_Comment by @MichaReiser on 2024-10-25 07:21_

Nice, thank you.
Would you mind adding a few tests showing the fix? Are there cases where a comprehension would be preferred over 
`list.extend`. If so, then I don't think we should provide a code fix.

---

_Label `fixes` added by @MichaReiser on 2024-10-25 07:21_

---

_Comment by @w0nder1ng on 2024-10-25 22:23_

Again, I'm not sure where to add tests for fixes. 

---

_Comment by @MichaReiser on 2024-10-26 07:52_

You can extend the existing tests. The testing framework runs your lint rule against the file and snapshots all created diagnostics with their fixes. 

https://github.com/astral-sh/ruff/blob/b34278e0cd25a24b69f4786897ca80c19fc3d5f1/crates/ruff_linter/src/rules/perflint/mod.rs#L20

---

_Comment by @github-actions[bot] on 2024-10-28 17:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+49 -49 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+31 -31 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L374'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:374:25:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L374'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:374:25:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3055'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3055:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3055'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3055:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L973'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:973:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L973'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:973:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/params/shell_params.py#L385'>dev/breeze/src/airflow_breeze/params/shell_params.py:385:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/params/shell_params.py#L385'>dev/breeze/src/airflow_breeze/params/shell_params.py:385:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/packages.py#L326'>dev/breeze/src/airflow_breeze/utils/packages.py:326:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/packages.py#L326'>dev/breeze/src/airflow_breeze/utils/packages.py:326:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/reproducible.py#L124'>dev/breeze/src/airflow_breeze/utils/reproducible.py:124:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/reproducible.py#L124'>dev/breeze/src/airflow_breeze/utils/reproducible.py:124:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1073'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1073:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1073'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1073:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L111'>docs/exts/docs_build/fetch_inventories.py:111:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L111'>docs/exts/docs_build/fetch_inventories.py:111:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L119'>docs/exts/docs_build/fetch_inventories.py:119:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L119'>docs/exts/docs_build/fetch_inventories.py:119:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L486'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:486:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L486'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:486:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L669'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:669:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L669'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:669:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py#L122'>providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py:122:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py#L122'>providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py:122:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L1210'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:1210:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L1210'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:1210:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L379'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:379:17:</a> PERF401 Use `list.extend` to create a transformed list
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/scripts/benchmark_migration.py#L128'>scripts/benchmark_migration.py:128:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/scripts/benchmark_migration.py#L128'>scripts/benchmark_migration.py:128:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/tasks/cache.py#L192'>superset/tasks/cache.py:192:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/tasks/cache.py#L192'>superset/tasks/cache.py:192:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/utils/core.py#L1184'>superset/utils/core.py:1184:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/utils/core.py#L1184'>superset/utils/core.py:1184:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L353'>tests/integration_tests/charts/api_tests.py:353:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L353'>tests/integration_tests/charts/api_tests.py:353:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L467'>tests/integration_tests/charts/api_tests.py:467:13:</a> PERF401 Use `list.extend` to create a transformed list
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L475'>src/bokeh/layouts.py:475:25:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L475'>src/bokeh/layouts.py:475:25:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L780'>src/bokeh/settings.py:780:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L780'>src/bokeh/settings.py:780:21:</a> PERF401 Use a list comprehension to create a transformed list
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 98 | 49 | 49 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+49 -49 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+31 -31 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L374'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:374:25:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L374'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:374:25:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3055'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3055:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3055'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3055:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L973'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:973:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L973'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:973:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/params/shell_params.py#L385'>dev/breeze/src/airflow_breeze/params/shell_params.py:385:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/params/shell_params.py#L385'>dev/breeze/src/airflow_breeze/params/shell_params.py:385:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/packages.py#L326'>dev/breeze/src/airflow_breeze/utils/packages.py:326:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/packages.py#L326'>dev/breeze/src/airflow_breeze/utils/packages.py:326:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/reproducible.py#L124'>dev/breeze/src/airflow_breeze/utils/reproducible.py:124:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/reproducible.py#L124'>dev/breeze/src/airflow_breeze/utils/reproducible.py:124:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1073'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1073:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1073'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1073:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L111'>docs/exts/docs_build/fetch_inventories.py:111:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L111'>docs/exts/docs_build/fetch_inventories.py:111:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L119'>docs/exts/docs_build/fetch_inventories.py:119:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/docs/exts/docs_build/fetch_inventories.py#L119'>docs/exts/docs_build/fetch_inventories.py:119:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L486'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:486:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L486'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:486:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L669'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:669:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/hooks/s3.py#L669'>providers/src/airflow/providers/amazon/aws/hooks/s3.py:669:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py#L122'>providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py:122:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py#L122'>providers/src/airflow/providers/amazon/aws/log/s3_task_handler.py:122:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L1210'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:1210:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L1210'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:1210:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/ccb18abdba26978c31db6bd3407689c73322273c/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L379'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py:379:17:</a> PERF401 Use `list.extend` to create a transformed list
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/scripts/benchmark_migration.py#L128'>scripts/benchmark_migration.py:128:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/scripts/benchmark_migration.py#L128'>scripts/benchmark_migration.py:128:21:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/tasks/cache.py#L192'>superset/tasks/cache.py:192:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/tasks/cache.py#L192'>superset/tasks/cache.py:192:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/utils/core.py#L1184'>superset/utils/core.py:1184:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/superset/utils/core.py#L1184'>superset/utils/core.py:1184:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L353'>tests/integration_tests/charts/api_tests.py:353:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L353'>tests/integration_tests/charts/api_tests.py:353:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/683ed0d943c3c3e9d6a25e64829eae7e7bc2b2bb/tests/integration_tests/charts/api_tests.py#L467'>tests/integration_tests/charts/api_tests.py:467:13:</a> PERF401 Use `list.extend` to create a transformed list
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L475'>src/bokeh/layouts.py:475:25:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L475'>src/bokeh/layouts.py:475:25:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L780'>src/bokeh/settings.py:780:21:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L780'>src/bokeh/settings.py:780:21:</a> PERF401 Use a list comprehension to create a transformed list
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 98 | 49 | 49 | 0 | 0 |

</p>
</details>




---

_Comment by @w0nder1ng on 2024-10-28 17:07_

One thing that I wasn't sure how to handle is cases where there is control flow between the binding and the loop.

```python
def f(early_return):
    result = []
    if early_return:
        return
    for i in range(10):
        result.append(i+1)
  ```

Currently, this fix offers to rewrite the code:

```python
def f(early_return):
    result = [i+1 for i in range(10)]
    if early_return:
        return
  ```

How do you think this case ought to be handled?

---

_Renamed from "[`PERF401`] implement quick-fix for `manual-list-comprehension` (`PERF401`)" to "[`perflint`] implement quick-fix for `manual-list-comprehension` (`PERF401`)" by @w0nder1ng on 2024-10-28 18:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:60 on 2024-10-29 17:29_

Let's adjust the message to make use of the new `comprehension_type` as well to avoid inconsistency between the message and the fix title.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:242 on 2024-10-29 17:52_

Nit: 
```suggestion
    let binding_has_one_target = {
        match binding_stmt.map(|binding_stmt| binding_stmt.targets.as_slice()) {
            Some([only_target]) => only_target.is_name_expr(),
            _ => false,
        }
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:248 on 2024-10-29 17:58_

Nit: the check for `binding_stmt.is_some` here seems unnecessary because `comprehension_type` can only be `List` when `binding_is_empty_list` is true. 

I suggest that we merge the `comprehension_type` definition on line 218 with the definition here:

```rust
    let comprehension_type = if binding_is_empty_list
        && assignment_in_same_statement
        && binding_has_one_target
    {
        ComprehensionType::ListComprehension
    } else {
        ComprehensionType::Extend
    };
```

This should also simplify the match statement inside of `try_set_optional_fix` because it's no longer necessary to match on `Option<ComprehensionType>`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:261 on 2024-10-30 06:53_

Let's gate the new fix behind preview `if checker.settings.preview.is_enabled() { ... }`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__PERF401_PERF401.py.snap`:21 on 2024-10-30 06:54_

The comment handling is a bit weird. Is there anything we can do to improve it?

---

_@MichaReiser requested changes on 2024-10-30 06:56_

This looks great. Thanks for working on this fix. 

I have a few smaller code improvements that are up to you. The one change we should make is to gate the fix behind preview to give us a chance to catch any errors before rolling it out to all users.

---

_@w0nder1ng reviewed on 2024-11-01 15:07_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__PERF401_PERF401.py.snap`:21 on 2024-11-01 15:07_

I also just noticed that comments which are inside of the for loop get deleted, so I'll see if I can modify it so it keeps those and fixes the whitespace

---

_@MichaReiser reviewed on 2024-11-01 15:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__PERF401_PERF401.py.snap`:21 on 2024-11-01 15:45_

That might be fine. I wouldn't spent too much time on it. The way we normally preserve comments is by not using generator and instead build the fix directly from source (by doing string manipulations)

---

_Comment by @w0nder1ng on 2024-11-03 21:49_

I've modified the code to preserve comments and not leave a newline when removing the for loop. The tests seem to be failing because the fix is now gated behind a preview--is there a way to allow the test to set the preview flag, or should I temporarily change the fix availability to "sometimes"?

---

_Comment by @dhruvmanila on 2024-11-04 03:20_

> The tests seem to be failing because the fix is now gated behind a preview--is there a way to allow the test to set the preview flag, or should I temporarily change the fix availability to "sometimes"?

@w0nder1ng You'll need to add a new test case where the preview flag is enabled. Refer to the following as an example:

https://github.com/astral-sh/ruff/blob/8d7dda9fb7066f02ebedc1e63a270861816c4c72/crates/ruff_linter/src/rules/ruff/mod.rs#L386-L402

---

_Comment by @w0nder1ng on 2024-11-04 16:00_

How should I deal with the existing test case? 

---

_Comment by @MichaReiser on 2024-11-05 07:16_

> How should I deal with the existing test case?

You would have to mark the rule as sometimes fixable. I suggest to add a `TODO` comment to the preview check that says that the rule should be changed to always fixable when promoting the fix to stable.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:68 on 2024-11-06 11:47_

Nit:

```suggestion
            Some(ComprehensionType::ListComprehension) | None => "comprehension",
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:233 on 2024-11-06 11:48_

Nit: We can avoid looking up the `for_loop_parent` if the `binding` is `None`

```suggestion
        binding.source.is_some_and(|binding_source| {
        	  let for_loop_parent = checker.semantic().current_statement_parent_id();
            let binding_parent = checker.semantic().parent_statement_id(binding_source);
            for_loop_parent == binding_parent
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:262 on 2024-11-06 11:48_

```suggestion
    // TODO: once this fix is stabilized, change the rule to always fixable
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:307 on 2024-11-06 11:53_

Nit

```suggestion
    let elt_str = locator.slice(to_append);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:294 on 2024-11-06 11:54_

Nit: Let's move this closer to where it's used (line 320). It should help with readability 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:72 on 2024-11-06 12:04_

The message with `extend` reads a bit off. `Use a list extend to create a transformed list`. It should be something like `Use `list.extend` to append to an existing list`

---

_@MichaReiser approved on 2024-11-06 12:09_

This is great

---

_Comment by @w0nder1ng on 2024-11-07 01:54_

I changed the diagnostic message, does the new one make more sense? Also, do you know why the tests aren't running?

---

_Comment by @dhruvmanila on 2024-11-07 04:06_

@w0nder1ng there are merge conflicts which needs to be resolved to get the CI running

---

_Label `preview` added by @MichaReiser on 2024-11-08 08:25_

---

_@MichaReiser approved on 2024-11-08 08:31_

This is great and very impressive comment handling! 

I pushed a small change that uses the improved message even in stable mode

---

_Comment by @MichaReiser on 2024-11-08 08:33_

Hmm, what's up with the ecosystem check... Let's re-run to get a better sense of the changes

---

_Comment by @MichaReiser on 2024-11-08 08:48_

I suspect that the ecosystem check is panicking during the fix generation. Let me run it locally

---

_@MichaReiser requested changes on 2024-11-08 08:56_

Here's an example that currently panics:

```py
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
"""migrate_old_annotation_layers

Revision ID: 21e88bc06c02
Revises: 67a6ac9b727b
Create Date: 2017-12-17 11:06:30.180267

"""

from alembic import op
from sqlalchemy import Column, Integer, or_, String, Text
from sqlalchemy.ext.declarative import declarative_base

from superset import db
from superset.utils import json

# revision identifiers, used by Alembic.
revision = "21e88bc06c02"
down_revision = "67a6ac9b727b"

Base = declarative_base()


class Slice(Base):
    __tablename__ = "slices"
    id = Column(Integer, primary_key=True)
    viz_type = Column(String(250))
    params = Column(Text)


def upgrade():
    bind = op.get_bind()
    session = db.Session(bind=bind)

    for slc in session.query(Slice).filter(
        or_(Slice.viz_type.like("line"), Slice.viz_type.like("bar"))
    ):
        params = json.loads(slc.params)
        layers = params.get("annotation_layers", [])
        if layers:
            new_layers = []
            for layer in layers:
                new_layers.append(
                    {
                        "annotationType": "INTERVAL",
                        "style": "solid",
                        "name": f"Layer {layer}",
                        "show": True,
                        "overrides": {"since": None, "until": None},
                        "value": layer,
                        "width": 1,
                        "sourceType": "NATIVE",
                    }
                )
            params["annotation_layers"] = new_layers
            slc.params = json.dumps(params)
            session.commit()
    session.close()


def downgrade():
    bind = op.get_bind()
    session = db.Session(bind=bind)

    for slc in session.query(Slice).filter(
        or_(Slice.viz_type.like("line"), Slice.viz_type.like("bar"))
    ):
        params = json.loads(slc.params)
        layers = params.get("annotation_layers", [])
        if layers:
            params["annotation_layers"] = [layer["value"] for layer in layers]
            slc.params = json.dumps(params)
            session.commit()
    session.close()

```

@w0nder1ng could you take a look at what's the source of the panic?

---

_Comment by @w0nder1ng on 2024-11-08 16:13_

The issue was that having no comments inside the for loop caused an empty insert, and that made a debug assert panic. The reason I didn't catch it before was because every test case for the fix has `# PERF401` following the lint target! I added a new test case to hopefully spot this type of error.

---

_Review requested from @MichaReiser by @w0nder1ng on 2024-11-08 21:17_

---

_@MichaReiser approved on 2024-11-11 11:16_

---

_Comment by @MichaReiser on 2024-11-11 11:16_

Ah, that makes sense. This is great. Thank you so much for working on the fix

---

_Merged by @MichaReiser on 2024-11-11 11:17_

---

_Closed by @MichaReiser on 2024-11-11 11:17_

---

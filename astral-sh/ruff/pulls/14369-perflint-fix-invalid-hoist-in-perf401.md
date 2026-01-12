```yaml
number: 14369
title: "[`perflint`] fix invalid hoist in `perf401`"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: perf401_invalid_hoist
created_at: 2024-11-15T21:03:52Z
updated_at: 2024-12-15T18:08:49Z
url: https://github.com/astral-sh/ruff/pull/14369
synced_at: 2026-01-12T15:55:47Z
```

# [`perflint`] fix invalid hoist in `perf401`

---

_@w0nder1ng_

This should fix #14362. This new fix currently deletes lines like this:
```python
- tmp = 1; result = []
- for i in range(10):
-   result.append(i+1)
+ result = [i+1 for i in range(10)]
```
Is there a convenient way to get every statement within a given `TextRange` to detect when this is happening?  

---

_Comment by @github-actions[bot] on 2024-11-15 21:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+18 -20 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3074'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3074:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3074'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3074:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/packages.py#L327'>dev/breeze/src/airflow_breeze/utils/packages.py:327:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/packages.py#L327'>dev/breeze/src/airflow_breeze/utils/packages.py:327:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/providers/src/airflow/providers/microsoft/azure/hooks/wasb.py#L721'>providers/src/airflow/providers/microsoft/azure/hooks/wasb.py:721:13:</a> PERF401 Use `list.extend` with an async comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/providers/src/airflow/providers/microsoft/azure/hooks/wasb.py#L721'>providers/src/airflow/providers/microsoft/azure/hooks/wasb.py:721:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/scripts/in_container/update_quarantined_test_status.py#L81'>scripts/in_container/update_quarantined_test_status.py:81:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/scripts/in_container/update_quarantined_test_status.py#L81'>scripts/in_container/update_quarantined_test_status.py:81:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L245'>superset/db_engine_specs/lib.py:245:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L245'>superset/db_engine_specs/lib.py:245:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L251'>superset/db_engine_specs/lib.py:251:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L251'>superset/db_engine_specs/lib.py:251:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L261'>superset/db_engine_specs/lib.py:261:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L261'>superset/db_engine_specs/lib.py:261:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L281'>superset/db_engine_specs/lib.py:281:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L281'>superset/db_engine_specs/lib.py:281:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L293'>superset/db_engine_specs/lib.py:293:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L293'>superset/db_engine_specs/lib.py:293:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/tasks/cache.py#L208'>superset/tasks/cache.py:208:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/tasks/cache.py#L208'>superset/tasks/cache.py:208:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/tests/integration_tests/security/migrate_roles_tests.py#L50'>tests/integration_tests/security/migrate_roles_tests.py:50:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/tests/integration_tests/security/migrate_roles_tests.py#L50'>tests/integration_tests/security/migrate_roles_tests.py:50:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/ldata/_transfer/upload.py#L163'>src/latch/ldata/_transfer/upload.py:163:25:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/ldata/_transfer/upload.py#L163'>src/latch/ldata/_transfer/upload.py:163:25:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/registry/utils.py#L70'>src/latch/registry/utils.py:70:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/registry/utils.py#L70'>src/latch/registry/utils.py:70:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/cp/utils.py#L54'>src/latch_cli/services/cp/utils.py:54:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/cp/utils.py#L54'>src/latch_cli/services/cp/utils.py:54:9:</a> PERF401 Use a list comprehension to create a transformed list
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
| PERF401 | 38 | 18 | 20 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -20 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3074'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3074:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3074'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3074:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py#L32'>dev/breeze/src/airflow_breeze/utils/exclude_from_matrix.py:32:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/packages.py#L327'>dev/breeze/src/airflow_breeze/utils/packages.py:327:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/dev/breeze/src/airflow_breeze/utils/packages.py#L327'>dev/breeze/src/airflow_breeze/utils/packages.py:327:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/docs/exts/docs_build/fetch_inventories.py#L103'>docs/exts/docs_build/fetch_inventories.py:103:9:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/providers/src/airflow/providers/microsoft/azure/hooks/wasb.py#L721'>providers/src/airflow/providers/microsoft/azure/hooks/wasb.py:721:13:</a> PERF401 Use `list.extend` with an async comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/providers/src/airflow/providers/microsoft/azure/hooks/wasb.py#L721'>providers/src/airflow/providers/microsoft/azure/hooks/wasb.py:721:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/scripts/in_container/update_quarantined_test_status.py#L81'>scripts/in_container/update_quarantined_test_status.py:81:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/3e2b5e2f54ea3b3695e6576313f87460fb098830/scripts/in_container/update_quarantined_test_status.py#L81'>scripts/in_container/update_quarantined_test_status.py:81:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/base.py#L107'>superset/db_engine_specs/base.py:107:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L245'>superset/db_engine_specs/lib.py:245:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L245'>superset/db_engine_specs/lib.py:245:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L251'>superset/db_engine_specs/lib.py:251:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L251'>superset/db_engine_specs/lib.py:251:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L261'>superset/db_engine_specs/lib.py:261:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L261'>superset/db_engine_specs/lib.py:261:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L281'>superset/db_engine_specs/lib.py:281:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L281'>superset/db_engine_specs/lib.py:281:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L293'>superset/db_engine_specs/lib.py:293:9:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/lib.py#L293'>superset/db_engine_specs/lib.py:293:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/tasks/cache.py#L208'>superset/tasks/cache.py:208:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/tasks/cache.py#L208'>superset/tasks/cache.py:208:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/tests/integration_tests/security/migrate_roles_tests.py#L50'>tests/integration_tests/security/migrate_roles_tests.py:50:13:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/tests/integration_tests/security/migrate_roles_tests.py#L50'>tests/integration_tests/security/migrate_roles_tests.py:50:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L479'>src/bokeh/plotting/_figure.py:479:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485'>src/bokeh/plotting/_figure.py:485:17:</a> PERF401 Use `list.extend` to create a transformed list
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L310'>src/bokeh/server/contexts.py:310:17:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/ldata/_transfer/upload.py#L163'>src/latch/ldata/_transfer/upload.py:163:25:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/ldata/_transfer/upload.py#L163'>src/latch/ldata/_transfer/upload.py:163:25:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/registry/utils.py#L70'>src/latch/registry/utils.py:70:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch/registry/utils.py#L70'>src/latch/registry/utils.py:70:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/cp/utils.py#L54'>src/latch_cli/services/cp/utils.py:54:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/cp/utils.py#L54'>src/latch_cli/services/cp/utils.py:54:9:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 38 | 18 | 20 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @charliermarsh on 2024-11-16 18:35_

---

_Label `fixes` added by @charliermarsh on 2024-11-16 18:35_

---

_Comment by @Skylion007 on 2024-11-17 17:25_

@w0nder1ng Another fun edge case that PR #14369 doesn't currently fix. Scoping rules are different between list comprehensions and for loops:

another example from pytorch
```diff
     if kwargs is None:
         kwargs = {}
-    impl_args = []
-    for a in args:
-        impl_args.append(_helper(a, map_fn))
+    impl_args = [_helper(a, map_fn) for a in args]
     impl_kwargs = {}
     for k in kwargs.keys():
         impl_kwargs[k] = _helper(a, map_fn)
```
the `a` on the line `impl_kwargs[k] = _helper(a, map_fn)` is now undefined (and will error with F821) since the list comprehension was converted from a loop. RUFF immeaditely flags this with an F821 error, and I test and confirm the temporary variable `a` does not leave the listcomp scope, while it does leave the for loop one. 

@w0nder1ng If you want a good test bed, just run the autofix on PyTorch (or another large codebase) and see after applying the fixes if any other ruff rule violations are immediately detected. 

---

_Comment by @Skylion007 on 2024-11-17 17:29_

Also another really minor nit is that it can duplicate comments.
```diff
-        for dtype in ["f16", "bf16"]:
-            kernels.append(
-                cls(
-                    aligned=True,
-                    dtype=dtype,
-                    sm_range=(80, SM[SM.index(80) + 1]),
-                    apply_dropout=False,
-                    preload_mmas=True,
-                    block_i=128,
-                    block_j=64,
-                    max_k=96,
-                    # Sm80 has a faster kernel for this case
-                    dispatch_cond="cc == 86 || cc == 89",
-                )
+        # Sm80 has a faster kernel for this case
+        kernels.extend(
+            cls(
+                aligned=True,
+                dtype=dtype,
+                sm_range=(80, SM[SM.index(80) + 1]),
+                apply_dropout=False,
+                preload_mmas=True,
+                block_i=128,
+                block_j=64,
+                max_k=96,
+                # Sm80 has a faster kernel for this case
+                dispatch_cond="cc == 86 || cc == 89",
             )
+            for dtype in ["f16", "bf16"]
+        )
```
See here.

---

_Comment by @w0nder1ng on 2024-11-17 22:02_

> Another fun edge case that PR https://github.com/astral-sh/ruff/pull/14369 doesn't currently fix

I think in this case, the lint shouldn't apply at all. If applying the fix breaks the code, then someone manually doing the same thing also wouldn't work. It should probably check that all references to the loop variable are inside the for loop before reporting the lint. 

I'll also try to fix the duplicate comments. I'm starting to see why this didn't have a fix before :)


---

_Comment by @w0nder1ng on 2024-11-18 02:22_

The comment duplication happened when the comment was inside the append body, it should be fixed now. 

---

_@w0nder1ng reviewed on 2024-11-18 02:27_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:237 on 2024-11-18 02:27_

For some reason, `resolve_name` returns `None` for the for-loop target. Also, references to it outside of the for-loop are not included in its list of references; e.g.
```python
def f():
    result = []
    for val in range(5):
        result.append(val * 2)
    print(val) # this reference is not included in the list of references
```

---

_@MichaReiser reviewed on 2024-11-18 07:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:237 on 2024-11-18 07:10_

I think the problem here is that we build the semantic model lazily as we traverse the AST for checking and it hasn't reached the point after the for loop yet

---

_Comment by @MichaReiser on 2024-11-18 07:14_

Could you take a look why https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L128 is no longer reported. I might have missed something obvious but it isn't clear to me why the diagnostic isn't reported anymore.

---

_@MichaReiser reviewed on 2024-11-18 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:237 on 2024-11-18 07:24_

@charliermarsh any suggestions on how to best find all usages of target?

---

_Comment by @w0nder1ng on 2024-11-18 15:27_

I suspect the regressions are because of the `resolve_name` currently being a `lookup_symbol`. In the example you gave, I think it's finding a different `foreign_key` binding on line 113 and concluding that the symbol is used outside of the for loop. Once the binding has the right scope, the only regressions should be ones where the lint shouldn't have applied.

---

_Comment by @Skylion007 on 2024-11-18 15:34_

> Could you take a look why https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L128 is no longer reported. I might have missed something obvious but it isn't clear to me why the diagnostic isn't reported anymore.

Because the lambda arg here https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L131C28-L131C33 is shadowing the variable "model" in the for loop. In this specific case, it's probably okay since it's in a lambda arg, but in general it shouldn't apply the fix there. I suppose only references rvalues are problematic outside of the forloop, not lvalues (if all they do is immediately get overwritten after all).

---

_Comment by @Skylion007 on 2024-11-18 15:49_

Okay hopefully last nit:
```diff
     reduction_axes: List[int] = []
-    for i in range(input_rank):
-        if i != axis:
-            reduction_axes.append(i)
+    reduction_axes.extend(i for i in range(input_rank) if i != axis)
```
Doesn't seem to like to hoist if there are any type_hints on the original `[]` instantiation.  Is this intentional?

---

_Comment by @w0nder1ng on 2024-11-18 15:52_

I'll take a look

---

_Comment by @w0nder1ng on 2024-11-18 16:18_

Annotated assigns have a different statement type than normal assigns, and I wasn't handling it. The fix should work on type-annotated lists now.

---

_Comment by @Skylion007 on 2024-11-18 21:20_

> @w0nder1ng Another fun edge case that PR #14369 doesn't currently fix. Scoping rules are different between list comprehensions and for loops:
> 
> another example from pytorch
> 
> ```diff
>      if kwargs is None:
>          kwargs = {}
> -    impl_args = []
> -    for a in args:
> -        impl_args.append(_helper(a, map_fn))
> +    impl_args = [_helper(a, map_fn) for a in args]
>      impl_kwargs = {}
>      for k in kwargs.keys():
>          impl_kwargs[k] = _helper(a, map_fn)
> ```
> 
> the `a` on the line `impl_kwargs[k] = _helper(a, map_fn)` is now undefined (and will error with F821) since the list comprehension was converted from a loop. RUFF immeaditely flags this with an F821 error, and I test and confirm the temporary variable `a` does not leave the listcomp scope, while it does leave the for loop one.
> 
> @w0nder1ng If you want a good test bed, just run the autofix on PyTorch (or another large codebase) and see after applying the fixes if any other ruff rule violations are immediately detected.

This one is still occurring sadly. Maybe because it references another loop lol?

---

_Comment by @Skylion007 on 2024-11-19 15:15_

Despite this minor false positive, looks like all the other fixes worked well on the PyTorch codebase (for the torch/torchgen folders) https://github.com/pytorch/pytorch/pull/140980/. 👍 

---

_Comment by @w0nder1ng on 2024-11-22 15:50_

I'm still stuck on two things.

1. How can I determine whether the binding statement has another statement on the line? 
```python
result = []; test = [] # deleting this whole line is wrong, but only deleting the binding statement also is wrong
# since it would leave the comment if there were no other statement
for ...
```

2. How can I get the semantic model to find references after the for loop? 
```python
result = []
for i in range(10):
  result.append(i+1)
print(i) # "fixing" this for loop is invalid because something relies on the loop variable, but the semantic model doesn't see this reference yet
```

---

_Comment by @Skylion007 on 2024-11-25 15:26_

@diceroll123 @MichaReiser Any idea how to tackle these edge cases?

---

_Comment by @MichaReiser on 2024-11-25 15:59_

@Skylion007 no, not from the top of my head and I haven't had time yet to look into it more closely

---

_Comment by @diceroll123 on 2024-11-25 16:04_

I did not look into it any further after my last comment on #14551.

This PR and my PR contain a bunch of overlapping changes (while addressing different bugs), and I don't want to step on any toes or spend more time on it if it's not going to be considered, so if one or the other is merged, or some kind of plan is made, we can go from there I suppose! No strong feelings on my part. 😄 

---

_Comment by @Skylion007 on 2024-11-28 17:07_

One other place this autofix breaks (but ruff does appropriately catch it!). Apply it to `sympy/tensor/index_methods.py` in sympy. Fixing this loop breaks: https://github.com/sympy/sympy/blob/ba68537e73dd40fb34fa9c5aaf46c5a6e1ef12c7/sympy/tensor/index_methods.py#L427 . I suspect because it's using unusual python syntax to iterate over an implicit tuple.
```python
for d in dbase, dexp:
```
transforming that to a list comprehension likely breaks

I will say that even it's current form, this autofix is extremely useful, and the encountered edge cases are very rare in real code, and usually caught by other checks.

---

_Comment by @Skylion007 on 2024-12-03 14:13_

This unaccepted rule would likely have to tackle many of the same problems. Did they solve this edge case? https://github.com/astral-sh/ruff/pull/11769

---

_@w0nder1ng reviewed on 2024-12-03 19:18_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:237 on 2024-12-03 19:18_

I think a working version would look something like this:

```rust
let for_loop_read = checker.semantic().resolve_load(for_loop_target_name_expr);
let for_loop_binding = match for_loop_read {
    ReadResult::Resolved(id) => checker.semantic().binding(id),
    _ => unreachable!("for loop target must exist"),
};
```

Unfortunately, `resolve_load` requires an `&mut SemanticModel` and I don't see a way to get one from the checker. Am I missing something here?

---

_Comment by @w0nder1ng on 2024-12-03 20:13_

@Skylion007 the implicit tuple issue should be fixed now

@diceroll123 this should also fix the async generator issue, as well as not duplicating comments inside of the for loop's iterator. It'd be great if you could look over it, since I haven't worked with async python, but I think it accomplishes the same thing as yours

---

_Comment by @MichaReiser on 2024-12-03 21:55_

I haven't thought this through yet but I wonder if we should convert this rule to a [binding rule instead](https://github.com/astral-sh/ruff/blob/f3d8c023d3baa1d225ef3f2a59f7660f90627003/crates/ruff_linter/src/checkers/ast/analyze/bindings.rs#L26-L27). Binding rules run **after** Ruff visited the entire AST. This has the advantage that the semantic model is complete. 

---

_Comment by @w0nder1ng on 2024-12-06 00:24_

I think that's all of the edge cases, except for the one involving the loop variable. I'd be happy to try to convert the rule into a binding rule, but I think that might be outside of the scope of this PR.

---

_@charliermarsh reviewed on 2024-12-06 02:52_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:237 on 2024-12-06 02:52_

Sorry, I didn't look deeply, but often in those cases you need to make it a deferred rule, so it runs _after_ the semantic model has been built (e.g., run it in `deferred_for_loops`).

---

_Comment by @w0nder1ng on 2024-12-06 17:14_

Even after moving the lint into `deferred_for_loops`, it still doesn't properly find the loop variable:

```python
def f():
    result = []
    for i in range(5):
    #   ^ running checker.semantic().resolve_name() on this ExprName returns None.
        result.append(i * 2)
        #             ^ when I run binding.statement() on this binding, 
        #               it returns the "i = 1" statement, even though that is clearly wrong
    i = 1
  # ^ lookup_symbol returns this i
    print(i) 
```
Am I missing something here, or is this behavior incorrect? See the last commit for the code I used to get this result. 

---

_Comment by @dhruvmanila on 2024-12-09 04:16_

> ```python
>     for i in range(5):
>     #   ^ running checker.semantic().resolve_name() on this ExprName returns None.
> ```

That might be because `i` is itself the definition. What I see in the semantic model is that an entry in the `resolved_names` is created only by the `resolve_load` function which would work on the `ExprName` with the `ExprContext::Load` context. But, here the `i` variable that is the for target would have the `ExprContext::Store` context (https://play.ruff.rs/44c3909e-4f46-4a46-bd6e-36ea78223322) which is why I think you're getting `None` there.

---

_@MichaReiser reviewed on 2024-12-09 09:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:250 on 2024-12-09 09:17_

```suggestion

```

---

_Comment by @MichaReiser on 2024-12-09 09:54_

>   # ^ lookup_symbol returns this i

This is because the semantic model now captures the state after running the entire program. It's complete, and running `lookup_symbol` gives you the most recent binding for `i` after evaluating the entire function scope. That means you now have to do some extra work to get the for-loop target binding: 

```rust
    let last_target_binding = checker
        .semantic()
        .lookup_symbol(id)
        .expect("For loop to exist");

    let mut bindings = [last_target_binding].into_iter().chain(
        checker
            .semantic()
            .shadowed_bindings(checker.semantic().scope_id, last_target_binding)
            .filter_map(|shadowed| shadowed.same_scope().then_some(shadowed.shadowed_id())),
    );

    let target_binding = bindings
        .find_map(|binding_id| {
            let binding = checker.semantic().binding(binding_id);

            if dbg!(binding.statement(checker.semantic())).and_then(Stmt::as_for_stmt)
                == Some(for_stmt)
            {
                Some(binding)
            } else {
                None
            }
        })
        .expect("for target binding must exist");

    drop(bindings);
```

There might be a better way to detect if the binding belongs to the target that e.g. avoids picking up named expressions in the iterator part.


---

_@w0nder1ng reviewed on 2024-12-09 22:00_

---

_Review comment by @w0nder1ng on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:258 on 2024-12-09 22:00_

I made this a vector, but if there's enough references, it might make more sense for it to be a HashMap instead. What do you think?

---

_Comment by @MichaReiser on 2024-12-10 09:03_

It seems we're now skipping over the two `result.append` usages in https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485

Is this intentional? Should we change the implementation not to provide a fix but still the usage?

Edit: I think this is intentional because `kw` is shadowed.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:278 on 2024-12-10 09:14_

I don't understand what this code is doing. Would you mind adding a comment explaining why looking at the `shadowed_references` is necessary?

---

_@MichaReiser reviewed on 2024-12-10 09:14_

---

_Comment by @w0nder1ng on 2024-12-10 19:11_

> It seems we're now skipping over the two `result.append` usages in https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L485
> 
> Is this intentional? Should we change the implementation not to provide a fix but still the usage?
> 
> Edit: I think this is intentional because `kw` is shadowed.

It would be shadowed if there weren't a return. A smaller example:
```python
def f(switch):
    i = 1
#   ^ this is the `i` which actually gets returned
    if switch:
        items = [1, 2, 3, 4]
        result = []
        #   v if it weren't for the return, this `i` would be the binding
        for i in items:
            result.append(i + 1)
        # v unconditional return means that the binding is not actually relevant
        return result
    #       v fix thinks this `i` is the loop variable
    return [i]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:474 on 2024-12-11 12:49_

I'm leaning towards not providing a fix in this case to avoid the extra complexity. 

Either way, we should avoid parsing here. We can use the `SimpleTokenizer` if we need tokenization and can't just use the AST

---

_@MichaReiser reviewed on 2024-12-11 12:53_

Uff, this rule is complicated. Thanks for pushing through it

I made it through the detection logic and pushed a few smaller simplifications. I'm now about to review the fix and noticed the `parse` call. I suggest to either not provide a fix in the multi assignment case (it's not that common) or to use the `SimpleTokenizer` to avoid using the full parser here. It might even simplify some of your code because it allows e.g. searching for the `;`

---

_Comment by @w0nder1ng on 2024-12-11 21:28_

I've removed the tokenization without changing the rest of the code. The code now iterates over the characters directly because I was having trouble with the `SimpleTokenizer` not properly tokenizing some lines.

 Is it worth also removing the code to handle multiple statements to simplify the logic? 

---

_Comment by @MichaReiser on 2024-12-12 08:05_

> The code now iterates over the characters directly because I was having trouble with the SimpleTokenizer not properly tokenizing some lines. 

I pushed a change that uses the `SimpleTokenizer` and `BackwardsTokenizer` instead. That also revealed that we should remove any leading `;`. 

But I think we're good now and can land this change. Thank you for working on this very involved. rule

---

_Merged by @MichaReiser on 2024-12-12 08:11_

---

_Closed by @MichaReiser on 2024-12-12 08:11_

---

_Comment by @Skylion007 on 2024-12-15 17:49_

Beautiful job landing this rule @w0nder1ng . Any plans on tackling the dict comprehension version since the logic is very similar?

---

_Comment by @w0nder1ng on 2024-12-15 18:08_

Sure, when I have the opportunity 

---

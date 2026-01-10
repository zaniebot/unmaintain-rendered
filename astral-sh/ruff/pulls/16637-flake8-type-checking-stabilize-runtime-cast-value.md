```yaml
number: 16637
title: "[`flake8-type-checking`] Stabilize `runtime-cast-value` (`TC006`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/tc006-0.10
created_at: 2025-03-11T16:43:43Z
updated_at: 2025-03-12T18:54:04Z
url: https://github.com/astral-sh/ruff/pull/16637
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-type-checking`] Stabilize `runtime-cast-value` (`TC006`)

---

_Pull request opened by @ntBre on 2025-03-11 16:43_

Summary
--

Stabilizes TC006. The test was already in the right place.

Test Plan
--

No open issues or PRs. The last related [issue] was closed on 2025-02-09.

[issue]: https://github.com/astral-sh/ruff/issues/16037

---

_Label `rule` added by @ntBre on 2025-03-11 16:43_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 16:43_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 16:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc006-0.10)

### Merging #16637 will **degrade performances by 4.6%**

<sub>Comparing <code>brent/tc006-0.10</code> (998869c) with <code>micha/ruff-0.10</code> (78164ab)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc006-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @github-actions[bot] on 2025-03-11 16:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+840 -0 violations, +0 -0 fixes in 11 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/abc.py#L1242'>disnake/abc.py:1242:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/abc.py#L298'>disnake/abc.py:298:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/components.py#L766'>disnake/components.py:766:27:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/base_core.py#L211'>disnake/ext/commands/base_core.py:211:43:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/core.py#L1938'>disnake/ext/commands/core.py:1938:19:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/core.py#L1968'>disnake/ext/commands/core.py:1968:19:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/interactions/base.py#L221'>disnake/interactions/base.py:221:37:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/docs/extensions/fulltoc.py#L122'>docs/extensions/fulltoc.py:122:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_abc.py#L164'>tests/test_abc.py:164:30:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_abc.py#L165'>tests/test_abc.py:165:30:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+171 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:17:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/app.py#L94'>airflow/api_fastapi/core_api/app.py:94:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/dag_report.py#L54'>airflow/api_fastapi/core_api/routes/public/dag_report.py:54:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/dag_run.py#L251'>airflow/api_fastapi/core_api/routes/public/dag_run.py:251:33:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/plugins.py#L44'>airflow/api_fastapi/core_api/routes/public/plugins.py:44:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/task_instances.py#L303'>airflow/api_fastapi/core_api/routes/public/task_instances.py:303:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/tasks.py#L57'>airflow/api_fastapi/core_api/routes/public/tasks.py:57:20:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/api_fastapi/core_api/routes/public/tasks.py#L80'>airflow/api_fastapi/core_api/routes/public/tasks.py:80:17:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/cli/commands/remote_commands/task_command.py#L302'>airflow/cli/commands/remote_commands/task_command.py:302:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/720f4a356645153f9d4dc3295c358a8990fe8556/airflow/dag_processing/collection.py#L741'>airflow/dag_processing/collection.py:741:23:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 161 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+82 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/charts/api.py#L601'>superset/charts/api.py:601:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/charts/api.py#L735'>superset/charts/api.py:735:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/dashboard/filter_state/create.py#L39'>superset/commands/dashboard/filter_state/create.py:39:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/dashboard/filter_state/update.py#L36'>superset/commands/dashboard/filter_state/update.py:36:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/database/tables.py#L145'>superset/commands/database/tables.py:145:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/distributed_lock/get.py#L45'>superset/commands/distributed_lock/get.py:45:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/explore/get.py#L116'>superset/commands/explore/get.py:116:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/explore/get.py#L130'>superset/commands/explore/get.py:130:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/sql_lab/export.py#L101'>superset/commands/sql_lab/export.py:101:44:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/d8d4b75a11fd35fd6a8f918b0109c72cb62d8d53/superset/commands/sql_lab/results.py#L111'>superset/commands/sql_lab/results.py:111:44:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 72 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+60 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L428'>src/bokeh/core/serialization.py:428:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L537'>src/bokeh/core/serialization.py:537:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L539'>src/bokeh/core/serialization.py:539:53:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L541'>src/bokeh/core/serialization.py:541:53:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L543'>src/bokeh/core/serialization.py:543:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L545'>src/bokeh/core/serialization.py:545:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L547'>src/bokeh/core/serialization.py:547:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L549'>src/bokeh/core/serialization.py:549:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L551'>src/bokeh/core/serialization.py:551:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L553'>src/bokeh/core/serialization.py:553:58:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 50 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+210 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/beta_decorator.py#L146'>libs/core/langchain_core/_api/beta_decorator.py:146:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/beta_decorator.py#L220'>libs/core/langchain_core/_api/beta_decorator.py:220:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/beta_decorator.py#L231'>libs/core/langchain_core/_api/beta_decorator.py:231:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L219'>libs/core/langchain_core/_api/deprecation.py:219:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L232'>libs/core/langchain_core/_api/deprecation.py:232:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L253'>libs/core/langchain_core/_api/deprecation.py:253:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L267'>libs/core/langchain_core/_api/deprecation.py:267:35:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L314'>libs/core/langchain_core/_api/deprecation.py:314:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L321'>libs/core/langchain_core/_api/deprecation.py:321:35:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L341'>libs/core/langchain_core/_api/deprecation.py:341:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/_api/deprecation.py#L394'>libs/core/langchain_core/_api/deprecation.py:394:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/langchain-ai/langchain/blob/5237987643578a9b7f2f014b538671db1ce4e0a0/libs/core/langchain_core/callbacks/file.py#L34'>libs/core/langchain_core/callbacks/file.py:34:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/table.py#L511'>src/latch/registry/table.py:511:35:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/table.py#L688'>src/latch/registry/table.py:688:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/utils.py#L38'>src/latch/registry/utils.py:38:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/utils.py#L54'>src/latch/registry/utils.py:54:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/utils.py#L64'>src/latch/registry/utils.py:64:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/register/utils.py#L157'>src/latch_cli/services/register/utils.py:157:32:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/register/utils.py#L159'>src/latch_cli/services/register/utils.py:159:24:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+216 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_config/config.py#L778'>pandas/_config/config.py:778:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_config/localization.py#L159'>pandas/_config/localization.py:159:57:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_testing/_warnings.py#L124'>pandas/_testing/_warnings.py:124:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_testing/_warnings.py#L244'>pandas/_testing/_warnings.py:244:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_testing/asserters.py#L286'>pandas/_testing/asserters.py:286:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_testing/asserters.py#L779'>pandas/_testing/asserters.py:779:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/_testing/asserters.py#L783'>pandas/_testing/asserters.py:783:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/compat/numpy/function.py#L172'>pandas/compat/numpy/function.py:172:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/core/algorithms.py#L1290'>pandas/core/algorithms.py:1290:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/core/algorithms.py#L1292'>pandas/core/algorithms.py:1292:35:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/core/algorithms.py#L175'>pandas/core/algorithms.py:175:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/core/algorithms.py#L180'>pandas/core/algorithms.py:180:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 204 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L278'>src/poetry/inspection/lazy_wheel.py:278:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/src/scikit_build_core/hatch/plugin.py#L49'>src/scikit_build_core/hatch/plugin.py:49:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/src/scikit_build_core/settings/sources.py#L642'>src/scikit_build_core/settings/sources.py:642:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/src/scikit_build_core/settings/sources.py#L643'>src/scikit_build_core/settings/sources.py:643:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/src/scikit_build_core/settings/sources.py#L644'>src/scikit_build_core/settings/sources.py:644:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/tests/test_builder.py#L108'>tests/test_builder.py:108:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/tests/test_builder.py#L128'>tests/test_builder.py:128:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/tests/test_builder.py#L141'>tests/test_builder.py:141:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/73ba800de2ef71a94147375a1af85097d9dd41c4/tests/test_pyproject_pep660.py#L14'>tests/test_pyproject_pep660.py:14:24:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+43 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/analytics/views/stats.py#L453'>analytics/views/stats.py:453:13:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/confirmation/models.py#L155'>confirmation/models.py:155:20:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/lib/remote_billing_util.py#L109'>corporate/lib/remote_billing_util.py:109:9:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/tests/test_stripe.py#L540'>corporate/tests/test_stripe.py:540:13:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/tests/test_stripe.py#L6363'>corporate/tests/test_stripe.py:6363:38:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/tests/test_stripe.py#L6463'>corporate/tests/test_stripe.py:6463:38:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/tests/test_stripe.py#L6973'>corporate/tests/test_stripe.py:6973:17:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/corporate/views/remote_billing_page.py#L458'>corporate/views/remote_billing_page.py:458:23:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/zerver/data_import/slack_message_conversion.py#L298'>zerver/data_import/slack_message_conversion.py:298:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/zulip/zulip/blob/a2892e713c13d34fb32467422f194094830b44f8/zerver/lib/addressee.py#L139'>zerver/lib/addressee.py:139:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 33 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC006 | 840 | 840 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 16:55_

I only spot-checked the ecosystem results here since there are 839 :sweat_smile: The ones I saw looked like true positives. I'm just a bit confused about the +0 fixes since this should always be fixable, and none of the examples I checked had comments to make it unsafe.

---

_Comment by @Daverball on 2025-03-12 07:17_

> I only spot-checked the ecosystem results here since there are 839 😅 The ones I saw looked like true positives. I'm just a bit confused about the +0 fixes since this should always be fixable, and none of the examples I checked had comments to make it unsafe.

I think the fixes column has been broken for quite a while now, even in my original PRs for implementing TC006-008 back in November the ecosystem results showed no fixes. I never have personally experienced it showing anything other than +0 -0 fixes in any of my PRS since either.

Perhaps the concise output formatting was changed at some point or a change to the supplied command line parameters meant that fixes are never part of the output. Maybe it would be more reliable to use a machine readable output format like json for `ruff-ecosystem`.

---

_Comment by @MichaReiser on 2025-03-12 07:47_

> I only spot-checked the ecosystem results here since there are 839 😅 The ones I saw looked like true positives. I'm just a bit confused about the +0 fixes since this should always be fixable, and none of the examples I checked had comments to make it unsafe.

It's a bit confusing :) The ecosystem check first matches diagnostics between runs and then computes how many new or no longer existing fixes there are. That means, fixes is a diff between existing diagnostics. It doesn't account for new diagnostics. 

You can expand the diagnostics and each one with a `[*]` has a fix. Clicking through, it seems almost all of them have a fix.

---

_@MichaReiser approved on 2025-03-12 07:48_

---

_Merged by @ntBre on 2025-03-12 18:54_

---

_Closed by @ntBre on 2025-03-12 18:54_

---

_Branch deleted on 2025-03-12 18:54_

---

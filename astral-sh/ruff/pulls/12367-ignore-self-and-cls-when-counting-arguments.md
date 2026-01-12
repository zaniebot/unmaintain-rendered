```yaml
number: 12367
title: Ignore self and cls when counting arguments
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/self-cls
created_at: 2024-07-17T14:33:28Z
updated_at: 2024-07-17T16:37:10Z
url: https://github.com/astral-sh/ruff/pull/12367
synced_at: 2026-01-12T15:55:41Z
```

# Ignore self and cls when counting arguments

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12320.


---

_Label `rule` added by @charliermarsh on 2024-07-17 14:33_

---

_Comment by @github-actions[bot] on 2024-07-17 14:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1650 -2208 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1544 -2031 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/api_client.py#L34'>airflow/api/client/api_client.py:34:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/json_client.py#L60'>airflow/api/client/json_client.py:60:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/local_client.py#L32'>airflow/api/client/local_client.py:32:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/commands/webserver_command.py#L85'>airflow/cli/commands/webserver_command.py:85:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/commands/webserver_command.py#L85'>airflow/cli/commands/webserver_command.py:85:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1062'>airflow/configuration.py:1062:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1062'>airflow/configuration.py:1062:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1083'>airflow/configuration.py:1083:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1083'>airflow/configuration.py:1083:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1104'>airflow/configuration.py:1104:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1104'>airflow/configuration.py:1104:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1126'>airflow/configuration.py:1126:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1126'>airflow/configuration.py:1126:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1393'>airflow/configuration.py:1393:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1393'>airflow/configuration.py:1393:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L553'>airflow/configuration.py:553:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L553'>airflow/configuration.py:553:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L612'>airflow/configuration.py:612:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L612'>airflow/configuration.py:612:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L639'>airflow/configuration.py:639:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L639'>airflow/configuration.py:639:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L122'>airflow/dag_processing/manager.py:122:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L122'>airflow/dag_processing/manager.py:122:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L363'>airflow/dag_processing/manager.py:363:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L363'>airflow/dag_processing/manager.py:363:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/processor.py#L84'>airflow/dag_processing/processor.py:84:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/datasets/manager.py#L71'>airflow/datasets/manager.py:71:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L171'>airflow/decorators/__init__.pyi:171:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L171'>airflow/decorators/__init__.pyi:171:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L353'>airflow/decorators/__init__.pyi:353:9:</a> PLR0913 Too many arguments in function definition (46 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L353'>airflow/decorators/__init__.pyi:353:9:</a> PLR0913 Too many arguments in function definition (47 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L501'>airflow/decorators/__init__.pyi:501:9:</a> PLR0913 Too many arguments in function definition (59 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L501'>airflow/decorators/__init__.pyi:501:9:</a> PLR0913 Too many arguments in function definition (60 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/base.py#L191'>airflow/decorators/base.py:191:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/base_executor.py#L156'>airflow/executors/base_executor.py:156:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/base_executor.py#L156'>airflow/executors/base_executor.py:156:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/debug_executor.py#L96'>airflow/executors/debug_executor.py:96:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/debug_executor.py#L96'>airflow/executors/debug_executor.py:96:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L115'>airflow/jobs/backfill_job_runner.py:115:9:</a> PLR0913 Too many arguments in function definition (17 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L115'>airflow/jobs/backfill_job_runner.py:115:9:</a> PLR0913 Too many arguments in function definition (18 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L846'>airflow/jobs/backfill_job_runner.py:846:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
... 3529 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+38 -61 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/async_events/async_query_manager.py#L186'>superset/async_events/async_query_manager.py:186:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/database/uploaders/base.py#L133'>superset/commands/database/uploaders/base.py:133:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/sql_lab/execute.py#L70'>superset/commands/sql_lab/execute.py:70:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/sql_lab/execute.py#L70'>superset/commands/sql_lab/execute.py:70:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context.py#L64'>superset/common/query_context.py:64:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context.py#L64'>superset/common/query_context.py:64:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_factory.py#L47'>superset/common/query_context_factory.py:47:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_factory.py#L47'>superset/common/query_context_factory.py:47:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_processor.py#L344'>superset/common/query_context_processor.py:344:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_object.py#L110'>superset/common/query_object.py:110:9:</a> PLR0913 Too many arguments in function definition (21 > 5)
... 89 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+20 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L85'>src/bokeh/client/connection.py:85:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L277'>src/bokeh/client/session.py:277:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/constraints.py#L56'>src/bokeh/core/property/constraints.py:56:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/container.py#L324'>src/bokeh/core/property/container.py:324:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L562'>src/bokeh/core/property/descriptors.py:562:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L660'>src/bokeh/core/property/descriptors.py:660:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L159'>src/bokeh/core/property/numeric.py:159:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L438'>src/bokeh/core/property/wrappers.py:438:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
... 48 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+48 -78 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/lib/counts.py#L91'>analytics/lib/counts.py:91:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/management/commands/populate_analytics_db.py#L47'>analytics/management/commands/populate_analytics_db.py:47:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/management/commands/populate_analytics_db.py#L47'>analytics/management/commands/populate_analytics_db.py:47:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L207'>analytics/tests/test_counts.py:207:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L226'>analytics/tests/test_counts.py:226:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L226'>analytics/tests/test_counts.py:226:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1614'>corporate/lib/stripe.py:1614:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1614'>corporate/lib/stripe.py:1614:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1711'>corporate/lib/stripe.py:1711:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1711'>corporate/lib/stripe.py:1711:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
... 116 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0913 | 3858 | 1650 | 2208 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1650 -2208 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1544 -2031 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/api_client.py#L34'>airflow/api/client/api_client.py:34:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/json_client.py#L60'>airflow/api/client/json_client.py:60:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/api/client/local_client.py#L32'>airflow/api/client/local_client.py:32:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/commands/webserver_command.py#L85'>airflow/cli/commands/webserver_command.py:85:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/cli/commands/webserver_command.py#L85'>airflow/cli/commands/webserver_command.py:85:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1062'>airflow/configuration.py:1062:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1062'>airflow/configuration.py:1062:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1083'>airflow/configuration.py:1083:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1083'>airflow/configuration.py:1083:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1104'>airflow/configuration.py:1104:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1104'>airflow/configuration.py:1104:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1126'>airflow/configuration.py:1126:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1126'>airflow/configuration.py:1126:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1393'>airflow/configuration.py:1393:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L1393'>airflow/configuration.py:1393:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L553'>airflow/configuration.py:553:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L553'>airflow/configuration.py:553:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L612'>airflow/configuration.py:612:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L612'>airflow/configuration.py:612:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L639'>airflow/configuration.py:639:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/configuration.py#L639'>airflow/configuration.py:639:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L122'>airflow/dag_processing/manager.py:122:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L122'>airflow/dag_processing/manager.py:122:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L363'>airflow/dag_processing/manager.py:363:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/manager.py#L363'>airflow/dag_processing/manager.py:363:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/dag_processing/processor.py#L84'>airflow/dag_processing/processor.py:84:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/datasets/manager.py#L71'>airflow/datasets/manager.py:71:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L171'>airflow/decorators/__init__.pyi:171:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L171'>airflow/decorators/__init__.pyi:171:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L353'>airflow/decorators/__init__.pyi:353:9:</a> PLR0913 Too many arguments in function definition (46 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L353'>airflow/decorators/__init__.pyi:353:9:</a> PLR0913 Too many arguments in function definition (47 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L501'>airflow/decorators/__init__.pyi:501:9:</a> PLR0913 Too many arguments in function definition (59 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/__init__.pyi#L501'>airflow/decorators/__init__.pyi:501:9:</a> PLR0913 Too many arguments in function definition (60 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/decorators/base.py#L191'>airflow/decorators/base.py:191:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/base_executor.py#L156'>airflow/executors/base_executor.py:156:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/base_executor.py#L156'>airflow/executors/base_executor.py:156:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/debug_executor.py#L96'>airflow/executors/debug_executor.py:96:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/executors/debug_executor.py#L96'>airflow/executors/debug_executor.py:96:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
+ <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L115'>airflow/jobs/backfill_job_runner.py:115:9:</a> PLR0913 Too many arguments in function definition (17 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L115'>airflow/jobs/backfill_job_runner.py:115:9:</a> PLR0913 Too many arguments in function definition (18 > 5)
- <a href='https://github.com/apache/airflow/blob/63662044583031fc27d98af02f2913d324245db0/airflow/jobs/backfill_job_runner.py#L846'>airflow/jobs/backfill_job_runner.py:846:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
... 3529 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+38 -61 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/async_events/async_query_manager.py#L186'>superset/async_events/async_query_manager.py:186:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/database/uploaders/base.py#L133'>superset/commands/database/uploaders/base.py:133:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/sql_lab/execute.py#L70'>superset/commands/sql_lab/execute.py:70:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/commands/sql_lab/execute.py#L70'>superset/commands/sql_lab/execute.py:70:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context.py#L64'>superset/common/query_context.py:64:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context.py#L64'>superset/common/query_context.py:64:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_factory.py#L47'>superset/common/query_context_factory.py:47:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_factory.py#L47'>superset/common/query_context_factory.py:47:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_context_processor.py#L344'>superset/common/query_context_processor.py:344:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/apache/superset/blob/245e1986c1c5b61d2d30fff8ceebe1cf5132dd58/superset/common/query_object.py#L110'>superset/common/query_object.py:110:9:</a> PLR0913 Too many arguments in function definition (21 > 5)
... 89 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+20 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L85'>src/bokeh/client/connection.py:85:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L277'>src/bokeh/client/session.py:277:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/constraints.py#L56'>src/bokeh/core/property/constraints.py:56:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/container.py#L324'>src/bokeh/core/property/container.py:324:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L562'>src/bokeh/core/property/descriptors.py:562:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L660'>src/bokeh/core/property/descriptors.py:660:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L159'>src/bokeh/core/property/numeric.py:159:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L438'>src/bokeh/core/property/wrappers.py:438:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
... 48 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+48 -78 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/lib/counts.py#L91'>analytics/lib/counts.py:91:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/management/commands/populate_analytics_db.py#L47'>analytics/management/commands/populate_analytics_db.py:47:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/management/commands/populate_analytics_db.py#L47'>analytics/management/commands/populate_analytics_db.py:47:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L207'>analytics/tests/test_counts.py:207:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L226'>analytics/tests/test_counts.py:226:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/analytics/tests/test_counts.py#L226'>analytics/tests/test_counts.py:226:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1614'>corporate/lib/stripe.py:1614:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1614'>corporate/lib/stripe.py:1614:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1711'>corporate/lib/stripe.py:1711:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/corporate/lib/stripe.py#L1711'>corporate/lib/stripe.py:1711:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
... 116 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0913 | 3858 | 1650 | 2208 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-07-17 14:49_

---

_Closed by @charliermarsh on 2024-07-17 14:49_

---

_Branch deleted on 2024-07-17 14:49_

---

_Comment by @dhruvmanila on 2024-07-17 16:34_

Should this change go under preview? I think most changes in the ecosystem should cancel each other out (just a decrease in the count) but it seems like there are lot of violations in addition to that.

---

_Comment by @charliermarsh on 2024-07-17 16:37_

No (IMO), because it only reduces the number of violations.

---

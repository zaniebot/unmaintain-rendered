```yaml
number: 14438
title: "[ruff 0.8][`flake8-type-checking`] Rename `TCH` to `TC`"
type: pull_request
state: merged
author: Daverball
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.8
head: rename-tch-to-tc
created_at: 2024-11-18T17:15:45Z
updated_at: 2024-11-28T15:29:25Z
url: https://github.com/astral-sh/ruff/pull/14438
synced_at: 2026-01-12T15:55:47Z
```

# [ruff 0.8][`flake8-type-checking`] Rename `TCH` to `TC`

---

_@Daverball_

Closes #9573

## Summary

This renames `TCH` to `TC` to match the original flake8 plugin.

## Test Plan

`cargo nextest run`


---

_Review requested from @carljm by @Daverball on 2024-11-18 17:15_

---

_Review requested from @MichaReiser by @Daverball on 2024-11-18 17:15_

---

_Review requested from @AlexWaygood by @Daverball on 2024-11-18 17:15_

---

_Review requested from @sharkdp by @Daverball on 2024-11-18 17:15_

---

_Review request for @carljm removed by @AlexWaygood on 2024-11-18 17:16_

---

_Review request for @sharkdp removed by @AlexWaygood on 2024-11-18 17:16_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 17:18_

---

_Label `breaking` added by @MichaReiser on 2024-11-18 17:24_

---

_Label `configuration` added by @MichaReiser on 2024-11-18 17:24_

---

_@MichaReiser approved on 2024-11-18 17:25_

Awesome, thank you!

---

_Comment by @github-actions[bot] on 2024-11-18 17:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+746 -701 violations, +0 -0 fixes in 8 projects; 1 project error; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+115 -111 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/__main__.py#L25'>airflow/__main__.py:25:22:</a> TC003 Move standard library import `argparse.Namespace` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/__main__.py#L25'>airflow/__main__.py:25:22:</a> TCH003 Move standard library import `argparse.Namespace` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_connexion/schemas/asset_schema.py#L19'>airflow/api_connexion/schemas/asset_schema.py:19:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_connexion/schemas/asset_schema.py#L19'>airflow/api_connexion/schemas/asset_schema.py:19:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/parameters.py#L21'>airflow/api_fastapi/common/parameters.py:21:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/parameters.py#L21'>airflow/api_fastapi/common/parameters.py:21:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/router.py#L20'>airflow/api_fastapi/common/router.py:20:18:</a> TC003 Move standard library import `enum.Enum` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/router.py#L20'>airflow/api_fastapi/common/router.py:20:18:</a> TCH003 Move standard library import `enum.Enum` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/types.py#L19'>airflow/api_fastapi/common/types.py:19:22:</a> TC003 Move standard library import `datetime.timedelta` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/types.py#L19'>airflow/api_fastapi/common/types.py:19:22:</a> TCH003 Move standard library import `datetime.timedelta` into a type-checking block
... 216 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+321 -321 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/advanced_data_type/api.py#L28'>superset/advanced_data_type/api.py:28:47:</a> TC001 Move application import `superset.advanced_data_type.types.AdvancedDataTypeResponse` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/advanced_data_type/api.py#L28'>superset/advanced_data_type/api.py:28:47:</a> TCH001 Move application import `superset.advanced_data_type.types.AdvancedDataTypeResponse` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/async_events/async_query_manager.py#L26'>superset/async_events/async_query_manager.py:26:41:</a> TC002 Move third-party import `flask_caching.backends.base.BaseCache` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/async_events/async_query_manager.py#L26'>superset/async_events/async_query_manager.py:26:41:</a> TCH002 Move third-party import `flask_caching.backends.base.BaseCache` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/charts/data/api.py#L43'>superset/charts/data/api.py:43:45:</a> TC001 Move application import `superset.connectors.sqla.models.BaseDatasource` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/charts/data/api.py#L43'>superset/charts/data/api.py:43:45:</a> TCH001 Move application import `superset.connectors.sqla.models.BaseDatasource` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/charts/data/api.py#L47'>superset/charts/data/api.py:47:37:</a> TC001 Move application import `superset.models.sql_lab.Query` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/charts/data/api.py#L47'>superset/charts/data/api.py:47:37:</a> TCH001 Move application import `superset.models.sql_lab.Query` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/cli/test_db.py#L45'>superset/cli/test_db.py:45:43:</a> TC001 Move application import `superset.db_engine_specs.base.BaseEngineSpec` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/cli/test_db.py#L45'>superset/cli/test_db.py:45:43:</a> TCH001 Move application import `superset.db_engine_specs.base.BaseEngineSpec` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/create.py#L18'>superset/commands/annotation_layer/annotation/create.py:18:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/create.py#L18'>superset/commands/annotation_layer/annotation/create.py:18:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/create.py#L23'>superset/commands/annotation_layer/annotation/create.py:23:25:</a> TC002 Move third-party import `marshmallow.ValidationError` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/create.py#L23'>superset/commands/annotation_layer/annotation/create.py:23:25:</a> TCH002 Move third-party import `marshmallow.ValidationError` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/delete.py#L27'>superset/commands/annotation_layer/annotation/delete.py:27:41:</a> TC001 Move application import `superset.models.annotations.Annotation` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/delete.py#L27'>superset/commands/annotation_layer/annotation/delete.py:27:41:</a> TCH001 Move application import `superset.models.annotations.Annotation` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L18'>superset/commands/annotation_layer/annotation/update.py:18:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L18'>superset/commands/annotation_layer/annotation/update.py:18:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L23'>superset/commands/annotation_layer/annotation/update.py:23:25:</a> TC002 Move third-party import `marshmallow.ValidationError` into a type-checking block
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L23'>superset/commands/annotation_layer/annotation/update.py:23:25:</a> TCH002 Move third-party import `marshmallow.ValidationError` into a type-checking block
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L35'>superset/commands/annotation_layer/annotation/update.py:35:41:</a> TC001 Move application import `superset.models.annotations.Annotation` into a type-checking block
... 137 additional changes omitted for rule TC001
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/superset/commands/annotation_layer/annotation/update.py#L35'>superset/commands/annotation_layer/annotation/update.py:35:41:</a> TCH001 Move application import `superset.models.annotations.Annotation` into a type-checking block
... 137 additional changes omitted for rule TCH001
... 620 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+232 -232 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L18'>release/build.py:18:21:</a> TC001 Move application import `.config.Config` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L18'>release/build.py:18:21:</a> TCH001 Move application import `.config.Config` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L19'>release/build.py:19:21:</a> TC001 Move application import `.system.System` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L19'>release/build.py:19:21:</a> TCH001 Move application import `.system.System` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L19'>release/checks.py:19:21:</a> TC001 Move application import `.config.Config` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L19'>release/checks.py:19:21:</a> TCH001 Move application import `.config.Config` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L20'>release/checks.py:20:23:</a> TC001 Move application import `.pipeline.StepType` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L20'>release/checks.py:20:23:</a> TCH001 Move application import `.pipeline.StepType` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L21'>release/checks.py:21:21:</a> TC001 Move application import `.system.System` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L21'>release/checks.py:21:21:</a> TCH001 Move application import `.system.System` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L24'>release/credentials.py:24:21:</a> TC001 Move application import `.config.Config` into a type-checking block
... 189 additional changes omitted for rule TC001
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L24'>release/credentials.py:24:21:</a> TCH001 Move application import `.config.Config` into a type-checking block
... 189 additional changes omitted for rule TCH001
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L41'>src/bokeh/application/handlers/code.py:41:19:</a> TC003 Move standard library import `types.ModuleType` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L41'>src/bokeh/application/handlers/code.py:41:19:</a> TCH003 Move standard library import `types.ModuleType` into a type-checking block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L58'>src/bokeh/application/handlers/directory.py:58:19:</a> TC003 Move standard library import `types.ModuleType` into a type-checking block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L58'>src/bokeh/application/handlers/directory.py:58:19:</a> TCH003 Move standard library import `types.ModuleType` into a type-checking block
... 448 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+42 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/oracle/tests/test_client.py#L3'>ibis/backends/oracle/tests/test_client.py:3:36:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/polars/rewrites.py#L10'>ibis/backends/polars/rewrites.py:10:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/sql/rewrites.py#L17'>ibis/backends/sql/rewrites.py:17:57:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/sql/rewrites.py#L21'>ibis/backends/sql/rewrites.py:21:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/common/grounds.py#L29'>ibis/common/grounds.py:29:57:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/common/selectors.py#L8'>ibis/common/selectors.py:8:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/config.py#L3'>ibis/config.py:3:47:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/builders.py#L16'>ibis/expr/builders.py:16:53:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/builders.py#L17'>ibis/expr/builders.py:17:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/datatypes/tests/test_core.py#L3'>ibis/expr/datatypes/tests/test_core.py:3:26:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:38:</a> TC003 Move standard library import `multiprocessing.managers.DictProxy` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:38:</a> TCH003 Move standard library import `multiprocessing.managers.DictProxy` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:49:</a> TC003 Move standard library import `multiprocessing.managers.ListProxy` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:49:</a> TCH003 Move standard library import `multiprocessing.managers.ListProxy` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L12'>src/latch/ldata/_transfer/upload.py:12:19:</a> TC003 Move standard library import `queue.Queue` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L12'>src/latch/ldata/_transfer/upload.py:12:19:</a> TCH003 Move standard library import `queue.Queue` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/registry/record.py#L18'>src/latch/registry/record.py:18:49:</a> TC001 Move application import `latch.registry.upstream_types.types.DBType` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/registry/record.py#L18'>src/latch/registry/record.py:18:49:</a> TCH001 Move application import `latch.registry.upstream_types.types.DBType` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/register/utils.py#L5'>src/latch_cli/services/register/utils.py:5:8:</a> TC003 Move standard library import `io` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/register/utils.py#L5'>src/latch_cli/services/register/utils.py:5:8:</a> TCH003 Move standard library import `io` into a type-checking block
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+30 -30 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L36'>confirmation/models.py:36:9:</a> TC001 Move application import `zilencer.models.PreregistrationRemoteRealmBillingUser` into a type-checking block
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L36'>confirmation/models.py:36:9:</a> TCH001 Move application import `zilencer.models.PreregistrationRemoteRealmBillingUser` into a type-checking block
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L37'>confirmation/models.py:37:9:</a> TC001 Move application import `zilencer.models.PreregistrationRemoteServerBillingUser` into a type-checking block
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L37'>confirmation/models.py:37:9:</a> TCH001 Move application import `zilencer.models.PreregistrationRemoteServerBillingUser` into a type-checking block
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/actions/message_edit.py#L68'>zerver/actions/message_edit.py:68:30:</a> TC001 Move application import `zerver.lib.types.EditHistoryEvent` into a type-checking block
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/actions/message_edit.py#L68'>zerver/actions/message_edit.py:68:30:</a> TCH001 Move application import `zerver.lib.types.EditHistoryEvent` into a type-checking block
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/lib/bot_lib.py#L3'>zerver/lib/bot_lib.py:3:29:</a> TC003 Move standard library import `collections.abc.Callable` into a type-checking block
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/lib/bot_lib.py#L3'>zerver/lib/bot_lib.py:3:29:</a> TCH003 Move standard library import `collections.abc.Callable` into a type-checking block
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/lib/logging_util.py#L13'>zerver/lib/logging_util.py:13:25:</a> TC002 Move third-party import `django.http.HttpRequest` into a type-checking block
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/zerver/lib/logging_util.py#L13'>zerver/lib/logging_util.py:13:25:</a> TCH002 Move third-party import `django.http.HttpRequest` into a type-checking block
... 50 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>
<details><summary>Changes by rule (11 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC001 | 352 | 352 | 0 | 0 | 0 |
| TCH001 | 352 | 0 | 352 | 0 | 0 |
| TC003 | 198 | 198 | 0 | 0 | 0 |
| TCH003 | 198 | 0 | 198 | 0 | 0 |
| TC002 | 147 | 147 | 0 | 0 | 0 |
| TCH002 | 147 | 0 | 147 | 0 | 0 |
| RUF101 | 46 | 46 | 0 | 0 | 0 |
| D212 | 2 | 1 | 1 | 0 | 0 |
| TC005 | 2 | 2 | 0 | 0 | 0 |
| TCH005 | 2 | 0 | 2 | 0 | 0 |
| TCH004 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9086 -9061 violations, +0 -66 fixes in 10 projects; 1 project error; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6292 -6291 violations, +0 -58 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/__main__.py#L25'>airflow/__main__.py:25:22:</a> TC003 Move standard library import `argparse.Namespace` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/__main__.py#L25'>airflow/__main__.py:25:22:</a> TCH003 Move standard library import `argparse.Namespace` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/__init__.py#L43'>airflow/api/__init__.py:43:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/__init__.py#L44'>airflow/api/__init__.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/auth/backend/deny_all.py#L38'>airflow/api/auth/backend/deny_all.py:38:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/client/__init__.py#L27'>airflow/api/client/__init__.py:27:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/client/__init__.py#L38'>airflow/api/client/__init__.py:38:5:</a> DOC201 `return` is not documented in docstring
... 9021 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/delete_dag.py#L61'>airflow/api/common/delete_dag.py:61:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/delete_dag.py#L64'>airflow/api/common/delete_dag.py:64:15:</a> DOC501 Raised exception `DagNotFound` missing from docstring
... 2951 additional changes omitted for rule DOC501
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api/common/mark_tasks.py#L190'>airflow/api/common/mark_tasks.py:190:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_connexion/schemas/asset_schema.py#L19'>airflow/api_connexion/schemas/asset_schema.py:19:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_connexion/schemas/asset_schema.py#L19'>airflow/api_connexion/schemas/asset_schema.py:19:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/db/common.py#L33'>airflow/api_fastapi/common/db/common.py:33:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/db/common.py#L47'>airflow/api_fastapi/common/db/common.py:47:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/parameters.py#L21'>airflow/api_fastapi/common/parameters.py:21:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/parameters.py#L21'>airflow/api_fastapi/common/parameters.py:21:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/router.py#L20'>airflow/api_fastapi/common/router.py:20:18:</a> TC003 Move standard library import `enum.Enum` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/router.py#L20'>airflow/api_fastapi/common/router.py:20:18:</a> TCH003 Move standard library import `enum.Enum` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/types.py#L19'>airflow/api_fastapi/common/types.py:19:22:</a> TC003 Move standard library import `datetime.timedelta` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/common/types.py#L19'>airflow/api_fastapi/common/types.py:19:22:</a> TCH003 Move standard library import `datetime.timedelta` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/core_api/datamodels/assets.py#L20'>airflow/api_fastapi/core_api/datamodels/assets.py:20:22:</a> TC003 Move standard library import `datetime.datetime` into a type-checking block
... 105 additional changes omitted for rule TC003
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/api_fastapi/core_api/datamodels/assets.py#L20'>airflow/api_fastapi/core_api/datamodels/assets.py:20:22:</a> TCH003 Move standard library import `datetime.datetime` into a type-checking block
... 105 additional changes omitted for rule TCH003
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/assets/__init__.py#L128'>airflow/assets/__init__.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `value in ("self", "context")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/assets/__init__.py#L128'>airflow/assets/__init__.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `value in {"self", "context"}`.
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/assets/__init__.py#L238'>airflow/assets/__init__.py:238:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/assets/__init__.py#L243'>airflow/assets/__init__.py:243:9:</a> DOC402 `yield` is not documented in docstring
... 329 additional changes omitted for rule DOC402
+ <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/298f1fd73b500a70b48267160ad0d6bb258fd205/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
... 12607 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1491 -1493 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L104'>RELEASING/changelog.py:104:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L113'>RELEASING/changelog.py:113:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L54'>RELEASING/changelog.py:54:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC201 `return` is not documented in docstring
... 1824 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/scripts/benchmark_migration.py#L43'>scripts/benchmark_migration.py:43:5:</a> DOC501 Raised exception `Exception` missing from docstring
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/scripts/benchmark_migration.py#L51'>scripts/benchmark_migration.py:51:11:</a> DOC501 Raised exception `Exception` missing from docstring
- <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/scripts/cancel_github_workflows.py#L162'>scripts/cancel_github_workflows.py:162:5:</a> DOC501 Raised exception `ClickException` missing from docstring
+ <a href='https://github.com/apache/superset/blob/c6685a706d186d4fc0e7bc71d98995d1f2d893f1/scripts/cancel_github_workflows.py#L164'>scripts/cancel_github_workflows.py:164:15:</a> DOC501 Raised exception `ClickException` missing from docstring
... 2982 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/publish/publish_app_integ_base.py#L60'>tests/integration/publish/publish_app_integ_base.py:60:16:</a> PLR1714 Consider merging multiple comparisons: `f.suffix in (".yaml", ".json")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/publish/publish_app_integ_base.py#L60'>tests/integration/publish/publish_app_integ_base.py:60:16:</a> PLR1714 Consider merging multiple comparisons: `f.suffix in {".yaml", ".json"}`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+639 -638 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L107'>examples/advanced/extensions/parallel_plot/parallel_plot.py:107:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L15'>examples/advanced/extensions/parallel_plot/parallel_plot.py:15:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L53'>examples/basic/data/server_sent_events_source.py:53:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L60'>examples/basic/data/server_sent_events_source.py:60:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L16'>examples/interaction/js_callbacks/js_on_event.py:16:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L21'>examples/interaction/js_callbacks/js_on_event.py:21:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L83'>examples/models/daylight.py:83:12:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L33'>examples/models/gauges.py:33:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L34'>examples/models/gauges.py:34:5:</a> DOC201 `return` is not documented in docstring
... 395 additional changes omitted for rule DOC201
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L18'>release/build.py:18:21:</a> TC001 Move application import `.config.Config` into a type-checking block
... 1267 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+42 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/oracle/tests/test_client.py#L3'>ibis/backends/oracle/tests/test_client.py:3:36:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/polars/rewrites.py#L10'>ibis/backends/polars/rewrites.py:10:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/sql/rewrites.py#L17'>ibis/backends/sql/rewrites.py:17:57:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/backends/sql/rewrites.py#L21'>ibis/backends/sql/rewrites.py:21:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/common/grounds.py#L29'>ibis/common/grounds.py:29:57:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/common/selectors.py#L8'>ibis/common/selectors.py:8:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/config.py#L3'>ibis/config.py:3:47:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/builders.py#L16'>ibis/expr/builders.py:16:53:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/builders.py#L17'>ibis/expr/builders.py:17:50:</a> RUF101 [*] `TCH001` is a redirect to `TC001`
+ <a href='https://github.com/ibis-project/ibis/blob/abe378b46044ef48a3ed38d4a798c039cc028550/ibis/expr/datatypes/tests/test_core.py#L3'>ibis/expr/datatypes/tests/test_core.py:3:26:</a> RUF101 [*] `TCH003` is a redirect to `TC003`
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:38:</a> TC003 Move standard library import `multiprocessing.managers.DictProxy` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:38:</a> TCH003 Move standard library import `multiprocessing.managers.DictProxy` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:49:</a> TC003 Move standard library import `multiprocessing.managers.ListProxy` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L10'>src/latch/ldata/_transfer/upload.py:10:49:</a> TCH003 Move standard library import `multiprocessing.managers.ListProxy` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L12'>src/latch/ldata/_transfer/upload.py:12:19:</a> TC003 Move standard library import `queue.Queue` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/ldata/_transfer/upload.py#L12'>src/latch/ldata/_transfer/upload.py:12:19:</a> TCH003 Move standard library import `queue.Queue` into a type-checking block
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/registry/record.py#L18'>src/latch/registry/record.py:18:49:</a> TC001 Move application import `latch.registry.upstream_types.types.DBType` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/registry/record.py#L18'>src/latch/registry/record.py:18:49:</a> TCH001 Move application import `latch.registry.upstream_types.types.DBType` into a type-checking block
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/register/utils.py#L5'>src/latch_cli/services/register/utils.py:5:8:</a> TC003 Move standard library import `io` into a type-checking block
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/dtypes/dtypes.py#L76'>pandas/core/dtypes/dtypes.py:76:23:</a> TCH004 Move import `pyarrow` out of type-checking block. Import is used for more than type hinting.
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/groupby.py#L4069'>pandas/core/groupby/groupby.py:4069:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L1027'>pandas/io/html.py:1027:28:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>
<details><summary>Changes by rule (22 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 12117 | 6058 | 6059 | 0 | 0 |
| DOC501 | 3926 | 1963 | 1963 | 0 | 0 |
| DOC402 | 408 | 204 | 204 | 0 | 0 |
| TC001 | 352 | 352 | 0 | 0 | 0 |
| TCH001 | 352 | 0 | 352 | 0 | 0 |
| TC003 | 198 | 198 | 0 | 0 | 0 |
| TCH003 | 198 | 0 | 198 | 0 | 0 |
| DOC202 | 158 | 79 | 79 | 0 | 0 |
| TC002 | 147 | 147 | 0 | 0 | 0 |
| TCH002 | 147 | 0 | 147 | 0 | 0 |
| PYI041 | 68 | 2 | 0 | 0 | 66 |
| PLR1714 | 62 | 31 | 31 | 0 | 0 |
| RUF101 | 46 | 46 | 0 | 0 | 0 |
| PYI061 | 14 | 0 | 14 | 0 | 0 |
| RUF038 | 7 | 0 | 7 | 0 | 0 |
| DOC502 | 4 | 2 | 2 | 0 | 0 |
| TC005 | 2 | 2 | 0 | 0 | 0 |
| DOC403 | 2 | 1 | 1 | 0 | 0 |
| TCH005 | 2 | 0 | 2 | 0 | 0 |
| DTZ901 | 1 | 1 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |
| TCH004 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @MichaReiser on 2024-11-18 17:34_

---

_Closed by @MichaReiser on 2024-11-18 17:34_

---

_Branch deleted on 2024-11-28 15:29_

---

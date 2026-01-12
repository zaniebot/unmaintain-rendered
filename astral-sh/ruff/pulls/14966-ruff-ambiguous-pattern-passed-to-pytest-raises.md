```yaml
number: 14966
title: "[`ruff`] Ambiguous pattern passed to `pytest.raises()` (`RUF043`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PT201
created_at: 2024-12-14T00:27:08Z
updated_at: 2024-12-18T15:33:55Z
url: https://github.com/astral-sh/ruff/pull/14966
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Ambiguous pattern passed to `pytest.raises()` (`RUF043`)

---

_@InSyncWithFoo_

## Summary

Resolves #13705.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2024-12-14 00:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+503 -0 violations, +0 -0 fixes in 16 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L353'>tests/test_embeds.py:353:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L359'>tests/test_embeds.py:359:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L365'>tests/test_embeds.py:365:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L371'>tests/test_embeds.py:371:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L393'>tests/test_embeds.py:393:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L400'>tests/test_embeds.py:400:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L406'>tests/test_embeds.py:406:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_embeds.py#L412'>tests/test_embeds.py:412:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_flags.py#L163'>tests/test_flags.py:163:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_flags.py#L167'>tests/test_flags.py:167:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_migrate.py#L1066'>tests/core/test_migrate.py:1066:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_migrate.py#L1116'>tests/core/test_migrate.py:1116:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_migrate.py#L680'>tests/core/test_migrate.py:680:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_migrate.py#L720'>tests/core/test_migrate.py:720:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_migrate.py#L877'>tests/core/test_migrate.py:877:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_validation.py#L146'>tests/engine/test_validation.py:146:62:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_validation.py#L352'>tests/engine/test_validation.py:352:47:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_validation.py#L390'>tests/engine/test_validation.py:390:47:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_validation.py#L907'>tests/engine/test_validation.py:907:47:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L462'>tests/graph_components/validators/test_default_recipe_validator.py:462:54:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+114 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/kubernetes_tests/test_kubernetes_pod_operator.py#L631'>kubernetes_tests/test_kubernetes_pod_operator.py:631:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/kubernetes_tests/test_kubernetes_pod_operator.py#L671'>kubernetes_tests/test_kubernetes_pod_operator.py:671:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/auth_manager/avp/test_facade.py#L203'>providers/tests/amazon/aws/auth_manager/avp/test_facade.py:203:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/auth_manager/avp/test_facade.py#L246'>providers/tests/amazon/aws/auth_manager/avp/test_facade.py:246:37:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/auth_manager/avp/test_facade.py#L288'>providers/tests/amazon/aws/auth_manager/avp/test_facade.py:288:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_base_aws.py#L861'>providers/tests/amazon/aws/hooks/test_base_aws.py:861:46:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_base_aws.py#L882'>providers/tests/amazon/aws/hooks/test_base_aws.py:882:46:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_comprehend.py#L131'>providers/tests/amazon/aws/hooks/test_comprehend.py:131:37:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_datasync.py#L129'>providers/tests/amazon/aws/hooks/test_datasync.py:129:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_s3.py#L1406'>providers/tests/amazon/aws/hooks/test_s3.py:1406:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/1ce34ccca7075a85ec6be9d982f5b0a8bfa0f13b/providers/tests/amazon/aws/hooks/test_s3.py#L160'>providers/tests/amazon/aws/hooks/test_s3.py:160:17:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 103 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e788b858d07a8ddefeaa6933814dc025de9ed210/tests/integration_tests/db_engine_specs/hive_tests.py#L256'>tests/integration_tests/db_engine_specs/hive_tests.py:256:19:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L395'>tests/unit/bokeh/core/test_properties.py:395:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L398'>tests/unit/bokeh/core/test_properties.py:398:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L401'>tests/unit/bokeh/core/test_properties.py:401:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/clickhouse/tests/test_client.py#L424'>ibis/backends/clickhouse/tests/test_client.py:424:40:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/flink/tests/test_ddl.py#L141'>ibis/backends/flink/tests/test_ddl.py:141:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/impala/tests/test_exprs.py#L593'>ibis/backends/impala/tests/test_exprs.py:593:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/pyspark/tests/test_udf.py#L72'>ibis/backends/pyspark/tests/test_udf.py:72:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/tests/test_client.py#L330'>ibis/backends/tests/test_client.py:330:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/tests/test_generic.py#L449'>ibis/backends/tests/test_generic.py:449:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/tests/test_generic.py#L454'>ibis/backends/tests/test_generic.py:454:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/tests/test_vectorized_udf.py#L358'>ibis/backends/tests/test_vectorized_udf.py:358:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/backends/tests/test_vectorized_udf.py#L479'>ibis/backends/tests/test_vectorized_udf.py:479:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/ee976c4cc0e50a56f33fb67e596e5e92dfff28d5/ibis/common/tests/test_annotations.py#L251'>ibis/common/tests/test_annotations.py:251:32:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+178 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/apply/test_numba.py#L110'>pandas/tests/apply/test_numba.py:110:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/apply/test_numba.py#L116'>pandas/tests/apply/test_numba.py:116:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arithmetic/test_datetime64.py#L1857'>pandas/tests/arithmetic/test_datetime64.py:1857:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/boolean/test_construction.py#L189'>pandas/tests/arrays/boolean/test_construction.py:189:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/boolean/test_construction.py#L192'>pandas/tests/arrays/boolean/test_construction.py:192:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/boolean/test_construction.py#L30'>pandas/tests/arrays/boolean/test_construction.py:30:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/boolean/test_construction.py#L33'>pandas/tests/arrays/boolean/test_construction.py:33:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/categorical/test_analytics.py#L125'>pandas/tests/arrays/categorical/test_analytics.py:125:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/interval/test_overlaps.py#L66'>pandas/tests/arrays/interval/test_overlaps.py:66:55:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/string_/test_string.py#L296'>pandas/tests/arrays/string_/test_string.py:296:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/test_array.py#L518'>pandas/tests/arrays/test_array.py:518:26:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/arrays/test_datetimelike.py#L504'>pandas/tests/arrays/test_datetimelike.py:504:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/computation/test_eval.py#L1871'>pandas/tests/computation/test_eval.py:1871:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/config/test_config.py#L273'>pandas/tests/config/test_config.py:273:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/edf00e953e6e185345fbc488cd9a963ab2d59d58/pandas/tests/dtypes/cast/test_construct_object_arr.py#L19'>pandas/tests/dtypes/cast/test_construct_object_arr.py:19:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 161 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_env.py#L66'>tests/test_env.py:66:44:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_projectbuilder.py#L464'>tests/test_projectbuilder.py:464:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/tests/test_util.py#L35'>tests/test_util.py:35:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/9c75ea15c2f31a77e6043b80b1b7081372319d85/unit_test/projectfiles_test.py#L272'>unit_test/projectfiles_test.py:272:49:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pypa/cibuildwheel/blob/9c75ea15c2f31a77e6043b80b1b7081372319d85/unit_test/projectfiles_test.py#L277'>unit_test/projectfiles_test.py:277:40:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/packages/test_locker.py#L744'>tests/packages/test_locker.py:744:44:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/d8e988105fdff8c452e3cc73f7790db0b0b64c9d/tests/units/components/core/test_match.py#L202'>tests/units/components/core/test_match.py:202:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/reflex-dev/reflex/blob/d8e988105fdff8c452e3cc73f7790db0b0b64c9d/tests/units/components/core/test_match.py#L232'>tests/units/components/core/test_match.py:232:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/reflex-dev/reflex/blob/d8e988105fdff8c452e3cc73f7790db0b0b64c9d/tests/units/components/core/test_match.py#L307'>tests/units/components/core/test_match.py:307:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF043 | 503 | 503 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2024-12-14 00:38_

This is a really noisy rule. I checked some of the new violations; they all seem to be true positives.

The rule currently don't have a fix. Should I add an unsafe "Wrap in `re.escape()`"?

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-12-14 10:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:156 on 2024-12-14 15:25_

This is different to the rule I suggested in https://github.com/astral-sh/ruff/issues/13705#issuecomment-2532259079. I suggested having a rule that only emits a diagnostic if the string _contains_ a metacharacter. But your implementation only emits a diagnostic if the string _does not contain_ a metacharacter.

The reason I think it would be better to emit the error if the string _contains_ a metacharacter is that I think it's quite a common footgun to do things like this:

```py
import pytest

def do_thing_that_raises():
    raise Exception('foobar')

def test_error_message_contains_foo_at_end_of_sentence():
    with pytest.raises(Exception, match="foo."):
        do_thing_that_raises()
```

Where that `.` will match any character at all (since the string is interpreted as a regex, and `.` is a regex metacharacter). So the test would erroneously pass, when it should have failed. I think it's reasonable to say that the user should either make it explicit that they intend the character to be a regex metacharacter (by using a raw string -- `r"foo."`), or double-escape the metacharacter (`"foo\\."`), or wrap the call in `re.escape()` (`re.escape("foo.")`).

I think if you implement the rule I suggested, it will be much less noisy

---

_@AlexWaygood reviewed on 2024-12-14 15:25_

---

_@InSyncWithFoo reviewed on 2024-12-14 17:12_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:156 on 2024-12-14 17:12_

(Somehow I always miss the most important details.) Indeed, the number of violations reduced to only a quarter of what it was before (~500 vs. ~2000).

---

_Comment by @AlexWaygood on 2024-12-14 17:16_

This looks much better now, thank you! I feel like I'd prefer to have an example in the docs that uses a `.` character in the string, though. Some of these ecosystem hits are great examples of cases where they're almost certainly not testing _exactly_ what they wanted to test:
- https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/tests/test_flags.py#L167
- https://github.com/apache/superset/blob/092faa019b91b2da7b81de9b04446c34ce254f04/tests/integration_tests/db_engine_specs/hive_tests.py#L256
- https://github.com/apache/airflow/blob/5f13d0d416caa88f13bb061401088c44f6c0f2c6/providers/tests/amazon/aws/auth_manager/avp/test_facade.py#L288
- https://github.com/pypa/pip/blob/8936feef3bc181d04c16ee5ade9731d745ee4200/tests/unit/test_direct_url.py#L107

And I think using an unescaped `.` in the string is almost certainly the most common way in which people hit this bug-waiting-to-happen

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:156 on 2024-12-14 17:17_

```suggestion
/// of `pytest.raises()` where the string contains at least one unescaped
/// regex metacharacter.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:161 on 2024-12-14 17:18_

```suggestion
/// by prefixing the string with the `r` suffix, escaping the metacharacter(s)
/// in the string using backslashes, or wrapping the entire string in a call to
/// `re.escape()`.
```

---

_Comment by @InSyncWithFoo on 2024-12-14 17:22_

Done. Should we suggest an unsafe fix to go with it?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:185 on 2024-12-14 17:22_

````suggestion
/// ```python
/// import pytest
///
///
/// with pytest.raises(Exception, match="A full sentence."):
///     do_thing_that_raises()
/// ```
///
/// Use instead:
///
/// ```python
/// import pytest
///
///
/// with pytest.raises(Exception, match=r"A full sentence."):
///     do_thing_that_raises()
/// ```
///
/// Alternatively:
///
/// ```python
/// import pytest
/// import re
///
///
/// with pytest.raises(Exception, match=re.escape("A full sentence.")):
///     do_thing_that_raises()
/// ```
///
/// or:
///
/// ```python
/// import pytest
/// import re
///
///
/// with pytest.raises(Exception, "A full sentence\\."):
///     do_thing_that_raises()
/// ```
````

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:185 on 2024-12-14 17:23_

It would also be great to add a `## References` section at the bottom, with links to the docs for `pytest.raises()` and `re.escape()`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-14 17:24_

Can you add some tests that use strings where the metacharacters are "manually" escaped, such as

```py
with pytest.raises("A full sentence\\."):
    do_thing_that_raises()
```

We need to make sure we don't emit a diagnostic on things like that

---

_@AlexWaygood reviewed on 2024-12-14 17:24_

---

_Comment by @AlexWaygood on 2024-12-14 17:29_

> Done. Should we suggest an unsafe fix to go with it?

I'm not sure it's a good idea. Lots of the time it's pretty easy as a human to tell from looking at the string whether they intended the string to be a regex or a "plain" string. But I think it would be hard for Ruff to provide an accurate fix.

E.g. [here](https://github.com/pypa/setuptools/blob/cc43212ffcc2b3e9830fe4f95d465a17f9f04116/setuptools/tests/test_dist.py#L158) it's obvious that they _did_ want the string to be a regex, so the correct fix would be to add the `r` prefix to the string. But in [other](https://github.com/pypa/setuptools/blob/cc43212ffcc2b3e9830fe4f95d465a17f9f04116/setuptools/tests/config/test_apply_pyprojecttoml.py#L201) [hits](https://github.com/pypa/setuptools/blob/cc43212ffcc2b3e9830fe4f95d465a17f9f04116/setuptools/tests/config/test_pyprojecttoml_dynamic_deps.py#L87) in the same repo, it's pretty obvious that the correct fix would be to escape the character, either using backslashes or `re.escape()`. Whichever fix we provide, it might be more annoying than helpful to have Ruff suggest the autofix if it's going to be wrong ~50% of the time, even if it's an unsafe fix.

---

_@InSyncWithFoo reviewed on 2024-12-14 17:29_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-14 17:29_

This will require some big change, because we currently don't have a good model of regexes, just either they have some metacharacters or they don't. Writing an ad-hoc regex parser is something I'd like to avoid, at least for now.

---

_Comment by @InSyncWithFoo on 2024-12-14 17:30_

> Whichever fix we provide, it might be more annoying than helpful to have Ruff suggest the autofix if it's going to be wrong ~50% of the time, even if it's an unsafe fix.

Too bad we can only suggest one fix per violation. If that weren't a thing, letting the user choose would be a very good solution.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-14 17:36_

Couldn't we just use a similar approach to what `unnecessary-regular-expression` does for the `repl` argument of `re.sub()`? It just iterates through the characters using `tuple_windows()` method (which is from the `Itertools` crate):

https://github.com/astral-sh/ruff/blob/ac31b26a0ebc0f6070225eb1bde3bc81c65cf12a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs#L186-L198

So for each length-2 tuple, if you see `('\\', <metacharacter>)`, you know it's been escaped, so it won't cause a violation. But if you see `(<anything except for '\\'>, <metacharacter>)`, it's enough to trigger a violation. You iterate through the string using `tuple_windows()` until you find an `(<anything that's not '\\'>, <metacharacter>)` pair, then you can break and report a diagnostic. If you iterate through the whole string without finding a pair like that, you don't emit a diagnostic.

---

_@AlexWaygood reviewed on 2024-12-14 17:36_

---

_@InSyncWithFoo reviewed on 2024-12-14 17:43_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-14 17:43_

That seems good enough. I'll add "write a regex parser" to my bucket list, though.

---

_@AlexWaygood reviewed on 2024-12-14 17:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-14 17:44_

Hehe, looking forward to it! 😃

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-12-15 12:18_

---

_Label `rule` added by @AlexWaygood on 2024-12-15 15:26_

---

_Label `preview` added by @AlexWaygood on 2024-12-15 15:26_

---

_@AlexWaygood reviewed on 2024-12-17 14:50_

I'm not sure this should be a `PT` rule. We generally don't add a rule to a category corresponding to a pre-existing linter unless:
- the pre-existing linter also has the rule
- _or_ the linter has been publicly declared to be deprecated/ummaintained
- _or_ the linter has not seen any activity for >=2 years

Could you move this to the `RUF` category? (The other option would be to open an issue with flake8-pytest-style asking if they would be willing to add the rule)

---

_@AlexWaygood reviewed on 2024-12-17 15:13_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:145 on 2024-12-17 15:13_

it seems like you're no longer using this function from the other rule, so I think all changes to this file are unnecessary for this PR. Could you revert them?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT201.py`:1 on 2024-12-17 15:14_

Could you possibly add one or two tests that involve an escaped metacharacter followed by a metacharacter? I think your logic handles them correctly, I'd just like to be completist about things, e.g.

```py
pytest.raises(x, "foo\\.*bar")
```

and

```py
pytest.raises(x, "foo*\\.bar")
```

They should obviously both trigger the rule

---

_@AlexWaygood reviewed on 2024-12-17 15:14_

---

_@AlexWaygood reviewed on 2024-12-17 15:14_

Looks fantastic other than the comments I've just left. I think this is going to be a really useful rule!

---

_Renamed from "[`flake8-pytest-style`] Ambiguous pattern passed to `pytest.raises()` (`PT201`)" to "[`ruff`] Ambiguous pattern passed to `pytest.raises()` (`RUF043`)" by @InSyncWithFoo on 2024-12-17 18:57_

---

_Comment by @InSyncWithFoo on 2024-12-17 18:58_

@AlexWaygood Actually, it wasn't so fantastic. There was a major bug: The `last = next` line is not reached in many cases due to early `continue`s.

It was only because of your suggestion that I added more tests and thereby decided to also detect metasequences, which eventually lead to the discover of the bug. Thanks a lot!

---

_@MichaReiser reviewed on 2024-12-18 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/pytest_raises_ambiguous_pattern.rs`:107 on 2024-12-18 07:59_

This can be simplified to 
```suggestion
    let last = value.chars().next_back().unwrap_or('\x00');
```

It's probably cheaper than assigning in every single iteration and using `tuple_windows`

---

_@MichaReiser reviewed on 2024-12-18 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/pytest_raises_ambiguous_pattern.rs`:135 on 2024-12-18 08:02_

It's not clear to me why this can't be handled in the for-loop directly. What's special about the last character?

---

_@AlexWaygood reviewed on 2024-12-18 11:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/pytest_raises_ambiguous_pattern.rs`:135 on 2024-12-18 11:16_

it's because we iterate through the `chars` in overlapping pairs using `tuple_windows()`. The final tuple yielded by `tuple_windows()` will be `(<penultimate character>, <final character>)`, i.e., the `current` variable in the `for` loop will never hold the value of the final character of the string

---

_@AlexWaygood approved on 2024-12-18 11:49_

LGTM, thanks!

---

_Merged by @AlexWaygood on 2024-12-18 11:53_

---

_Closed by @AlexWaygood on 2024-12-18 11:53_

---

_Branch deleted on 2024-12-18 15:33_

---

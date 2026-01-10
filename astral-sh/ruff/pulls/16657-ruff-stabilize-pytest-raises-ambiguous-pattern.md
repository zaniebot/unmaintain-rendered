```yaml
number: 16657
title: "[`ruff`] Stabilize `pytest-raises-ambiguous-pattern` (`RUF043`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
base: micha/ruff-0.10
head: brent/ruf043-0.10
created_at: 2025-03-12T03:04:47Z
updated_at: 2025-05-01T15:30:54Z
url: https://github.com/astral-sh/ruff/pull/16657
synced_at: 2026-01-10T18:57:02Z
```

# [`ruff`] Stabilize `pytest-raises-ambiguous-pattern` (`RUF043`)

---

_Pull request opened by @ntBre on 2025-03-12 03:04_

Summary
--

Stabilizes RUF043 and moves its test to the right place. The docs look good.

Test Plan
--

0 closed or open issues. 1 follow-up PR to fix typos in the test documentation and 1 bug fix to treat `)` as a regex meta-character on 2025-01-07.

This can be a pretty noisy rule (+503 violations in the initial PR), so we'll see how the ecosystem check looks now. I'm happy to hold off stabilizing if we think it's too noisy, just wanted to open the PR just in case.


---

_Label `rule` added by @ntBre on 2025-03-12 03:04_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-12 03:04_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 03:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf043-0.10)

### Merging #16657 will **degrade performances by 10.81%**

<sub>Comparing <code>brent/ruf043-0.10</code> (4175c38) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf043-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 03:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+438 -0 violations, +0 -0 fixes in 14 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L353'>tests/test_embeds.py:353:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L359'>tests/test_embeds.py:359:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L365'>tests/test_embeds.py:365:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L371'>tests/test_embeds.py:371:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L393'>tests/test_embeds.py:393:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L400'>tests/test_embeds.py:400:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L406'>tests/test_embeds.py:406:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L412'>tests/test_embeds.py:412:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_flags.py#L163'>tests/test_flags.py:163:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_flags.py#L167'>tests/test_flags.py:167:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+10 -0 violations, +0 -0 fixes)</summary>
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
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+107 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/kubernetes_tests/test_kubernetes_pod_operator.py#L648'>kubernetes_tests/test_kubernetes_pod_operator.py:648:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/kubernetes_tests/test_kubernetes_pod_operator.py#L688'>kubernetes_tests/test_kubernetes_pod_operator.py:688:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py#L192'>providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py:192:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py#L235'>providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py:235:37:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py#L277'>providers/amazon/tests/unit/amazon/aws/auth_manager/avp/test_facade.py:277:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py#L856'>providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py:856:46:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py#L877'>providers/amazon/tests/unit/amazon/aws/hooks/test_base_aws.py:877:46:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_comprehend.py#L131'>providers/amazon/tests/unit/amazon/aws/hooks/test_comprehend.py:131:37:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_datasync.py#L129'>providers/amazon/tests/unit/amazon/aws/hooks/test_datasync.py:129:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L1391'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:1391:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L160'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:160:17:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L97'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:97:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 95 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/a3f3a35c2000771af5f4db474125d771ea722383/tests/integration_tests/db_engine_specs/hive_tests.py#L256'>tests/integration_tests/db_engine_specs/hive_tests.py:256:19:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L395'>tests/unit/bokeh/core/test_properties.py:395:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L398'>tests/unit/bokeh/core/test_properties.py:398:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_properties.py#L401'>tests/unit/bokeh/core/test_properties.py:401:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/clickhouse/tests/test_client.py#L424'>ibis/backends/clickhouse/tests/test_client.py:424:40:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/flink/tests/test_ddl.py#L141'>ibis/backends/flink/tests/test_ddl.py:141:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/impala/tests/test_exprs.py#L593'>ibis/backends/impala/tests/test_exprs.py:593:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/pyspark/tests/test_udf.py#L72'>ibis/backends/pyspark/tests/test_udf.py:72:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/tests/test_client.py#L349'>ibis/backends/tests/test_client.py:349:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/tests/test_generic.py#L455'>ibis/backends/tests/test_generic.py:455:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/tests/test_generic.py#L460'>ibis/backends/tests/test_generic.py:460:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/tests/test_vectorized_udf.py#L358'>ibis/backends/tests/test_vectorized_udf.py:358:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/backends/tests/test_vectorized_udf.py#L479'>ibis/backends/tests/test_vectorized_udf.py:479:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/ibis-project/ibis/blob/1045dbea46eacd86cfae5208aa7e6ef05b0312ca/ibis/common/tests/test_annotations.py#L251'>ibis/common/tests/test_annotations.py:251:32:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+177 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/apply/test_numba.py#L120'>pandas/tests/apply/test_numba.py:120:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/apply/test_numba.py#L126'>pandas/tests/apply/test_numba.py:126:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arithmetic/test_datetime64.py#L1857'>pandas/tests/arithmetic/test_datetime64.py:1857:34:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/boolean/test_construction.py#L189'>pandas/tests/arrays/boolean/test_construction.py:189:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/boolean/test_construction.py#L192'>pandas/tests/arrays/boolean/test_construction.py:192:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/boolean/test_construction.py#L30'>pandas/tests/arrays/boolean/test_construction.py:30:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/boolean/test_construction.py#L33'>pandas/tests/arrays/boolean/test_construction.py:33:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/categorical/test_analytics.py#L125'>pandas/tests/arrays/categorical/test_analytics.py:125:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/interval/test_overlaps.py#L66'>pandas/tests/arrays/interval/test_overlaps.py:66:55:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/string_/test_string.py#L299'>pandas/tests/arrays/string_/test_string.py:299:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/string_/test_string.py#L765'>pandas/tests/arrays/string_/test_string.py:765:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/test_array.py#L518'>pandas/tests/arrays/test_array.py:518:26:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/arrays/test_datetimelike.py#L504'>pandas/tests/arrays/test_datetimelike.py:504:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/computation/test_eval.py#L1871'>pandas/tests/computation/test_eval.py:1871:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/config/test_config.py#L273'>pandas/tests/config/test_config.py:273:48:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/dtypes/cast/test_construct_object_arr.py#L19'>pandas/tests/dtypes/cast/test_construct_object_arr.py:19:41:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/dtypes/test_common.py#L871'>pandas/tests/dtypes/test_common.py:871:43:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/3c93d06d641621ff62453c9c7748eeec3d6d8a97/pandas/tests/dtypes/test_dtypes.py#L388'>pandas/tests/dtypes/test_dtypes.py:388:45:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
... 157 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/e9adbf04f98745185ffb6552a7088a8ec12cfe2f/tests/test_env.py#L66'>tests/test_env.py:66:44:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pypa/build/blob/e9adbf04f98745185ffb6552a7088a8ec12cfe2f/tests/test_projectbuilder.py#L464'>tests/test_projectbuilder.py:464:52:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pypa/build/blob/e9adbf04f98745185ffb6552a7088a8ec12cfe2f/tests/test_util.py#L35'>tests/test_util.py:35:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/packages/test_locker.py#L744'>tests/packages/test_locker.py:744:44:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/c4729900cb7b59ce75bb577cbb73ad48c28c56c4/tests/units/components/core/test_match.py#L206'>tests/units/components/core/test_match.py:206:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/reflex-dev/reflex/blob/c4729900cb7b59ce75bb577cbb73ad48c28c56c4/tests/units/components/core/test_match.py#L236'>tests/units/components/core/test_match.py:236:15:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/reflex-dev/reflex/blob/c4729900cb7b59ce75bb577cbb73ad48c28c56c4/tests/units/components/core/test_match.py#L311'>tests/units/components/core/test_match.py:311:42:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF043 | 438 | 438 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-12 03:28_

These all look like true positives and almost all with `.` as the meta-character. There are many slightly incorrect cases matching sentences (`This is a sentence ending with period.`) or matching versions (`Python 3.10`) or object fields (`object.field is invalid`).

I'm not totally sure if the number of hits actually decreased or if the truncation is just different. airflow is down 7 violations but ibis and pandas are +1 each.

---

_Comment by @MichaReiser on 2025-03-12 08:35_

> I'm not totally sure if the number of hits actually decreased or if the truncation is just different. airflow is down 7 violations but ibis and pandas are +1 each.

Do you mean compared to the initial PR? It's possible that some repositories use preview mode and have fixed the violations in their code.

---

_@MichaReiser approved on 2025-03-12 08:43_

It's certainly a more noisy rule and it's unfortunate we can't provide an auto fix (because Ruff can't know what the desired behavior is). But I think it's fine to stabilize


Unless we want to add a heuristic for the auto fix. E.g.:

* A string ending with `.` is probably a sentence -> escape it
* A string with a sequence of metacharacters (e.g. `.*` or `.+`) is probably a regex -> make it raw. We can limit it to only support some specific patterns.

The first fix would have to be unsafe because it does change the program's semantics. The second one could be safe but I think it's still best to make user explicitly opt-in. We could document the heuristics in the rule and having a fix could reduce the churn for users. 

I'd postpone the promotion to stable if the suggested fix does sound reasonable (CC: @AlexWaygood)


---

_Comment by @ntBre on 2025-03-12 19:00_

I'm going to close this for now and open an issue with Micha's fix idea. The rule is a bit newer than most of the other stabilizations anyway.

I'm not opposed to reopening and merging it for v0.10, however. Just don't let me forget to add it back to the blog post!

---

_Closed by @ntBre on 2025-03-12 19:00_

---

_Branch deleted on 2025-05-01 15:30_

---

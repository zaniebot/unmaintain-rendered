```yaml
number: 14446
title: "[`ruff`] `re` and `regex` calls with unraw string as first argument (`RUF039`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF051
created_at: 2024-11-19T02:12:54Z
updated_at: 2024-11-20T03:46:02Z
url: https://github.com/astral-sh/ruff/pull/14446
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] `re` and `regex` calls with unraw string as first argument (`RUF039`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-19 02:12_

## Summary

Resolves #11167.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-19 03:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+199 -0 violations, +0 -0 fixes in 14 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L222'>rasa/utils/io.py:222:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L223'>rasa/utils/io.py:223:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L224'>rasa/utils/io.py:224:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L225'>rasa/utils/io.py:225:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L226'>rasa/utils/io.py:226:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L227'>rasa/utils/io.py:227:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L228'>rasa/utils/io.py:228:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L229'>rasa/utils/io.py:229:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L230'>rasa/utils/io.py:230:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L231'>rasa/utils/io.py:231:9:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+36 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/dev/breeze/src/airflow_breeze/params/build_prod_params.py#L81'>dev/breeze/src/airflow_breeze/params/build_prod_params.py:81:21:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/dev/breeze/src/airflow_breeze/utils/run_tests.py#L109'>dev/breeze/src/airflow_breeze/utils/run_tests.py:109:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/dev/perf/dags/elastic_dag.py#L73'>dev/perf/dags/elastic_dag.py:73:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/docs/exts/docs_build/lint_checks.py#L46'>docs/exts/docs_build/lint_checks.py:46:46:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/helm_tests/airflow_aux/test_pod_template_file.py#L358'>helm_tests/airflow_aux/test_pod_template_file.py:358:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/helm_tests/airflow_aux/test_pod_template_file.py#L370'>helm_tests/airflow_aux/test_pod_template_file.py:370:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/helm_tests/airflow_aux/test_pod_template_file.py#L407'>helm_tests/airflow_aux/test_pod_template_file.py:407:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/helm_tests/airflow_aux/test_pod_template_file.py#L59'>helm_tests/airflow_aux/test_pod_template_file.py:59:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/helm_tests/airflow_aux/test_pod_template_file.py#L97'>helm_tests/airflow_aux/test_pod_template_file.py:97:26:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/apache/airflow/blob/cc8aa7bc42e5ec9d1766e4b58297ce6f896de7ce/providers/src/airflow/providers/apache/spark/hooks/spark_submit.py#L602'>providers/src/airflow/providers/apache/spark/hooks/spark_submit.py:602:35:</a> RUF039 First argument to `re.search()` is not raw string
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+71 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/RELEASING/changelog.py#L276'>RELEASING/changelog.py:276:26:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/scripts/build_docker.py#L66'>scripts/build_docker.py:66:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/scripts/build_docker.py#L68'>scripts/build_docker.py:68:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/scripts/build_docker.py#L70'>scripts/build_docker.py:70:23:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/scripts/build_docker.py#L70'>scripts/build_docker.py:70:51:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/athena.py#L30'>superset/db_engine_specs/athena.py:30:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/bigquery.py#L76'>superset/db_engine_specs/bigquery.py:76:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/bigquery.py#L77'>superset/db_engine_specs/bigquery.py:77:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/bigquery.py#L90'>superset/db_engine_specs/bigquery.py:90:5:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L29'>superset/db_engine_specs/denodo.py:29:46:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L30'>superset/db_engine_specs/denodo.py:30:48:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L32'>superset/db_engine_specs/denodo.py:32:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L35'>superset/db_engine_specs/denodo.py:35:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L37'>superset/db_engine_specs/denodo.py:37:46:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L39'>superset/db_engine_specs/denodo.py:39:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L41'>superset/db_engine_specs/denodo.py:41:43:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/db_engine_specs/denodo.py#L43'>superset/db_engine_specs/denodo.py:43:9:</a> RUF039 First argument to `re.compile()` is not raw string
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/strings.py#L91'>src/bokeh/util/strings.py:91:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/strings.py#L92'>src/bokeh/util/strings.py:92:19:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_templates.py#L48'>tests/unit/bokeh/core/test_templates.py:48:19:</a> RUF039 First argument to `re.sub()` is not raw bytes literal
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L203'>tests/unit/bokeh/io/test_export.py:203:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L204'>tests/unit/bokeh/io/test_export.py:204:13:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L205'>tests/unit/bokeh/io/test_export.py:205:13:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L206'>tests/unit/bokeh/io/test_export.py:206:9:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/test_server__server.py#L211'>tests/unit/bokeh/server/test_server__server.py:211:28:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/test_server__server.py#L219'>tests/unit/bokeh/server/test_server__server.py:219:36:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/backends/__init__.py#L1396'>ibis/backends/__init__.py:1396:17:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/backends/flink/__init__.py#L320'>ibis/backends/flink/__init__.py:320:17:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/backends/sql/compilers/pyspark.py#L362'>ibis/backends/sql/compilers/pyspark.py:362:27:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/backends/tests/test_client.py#L1047'>ibis/backends/tests/test_client.py:1047:13:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/backends/tests/test_client.py#L1068'>ibis/backends/tests/test_client.py:1068:13:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/common/tests/test_patterns.py#L246'>ibis/common/tests/test_patterns.py:246:31:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/common/tests/test_patterns.py#L246'>ibis/common/tests/test_patterns.py:246:65:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/ibis-project/ibis/blob/db8af10a30fb204dd1dff25134e88a5d7433f4e0/ibis/tests/expr/test_selectors.py#L123'>ibis/tests/expr/test_selectors.py:123:39:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/centromere/ctx.py#L447'>src/latch_cli/centromere/ctx.py:447:26:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/init/init.py#L309'>src/latch_cli/services/init/init.py:309:18:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/init/init.py#L316'>src/latch_cli/services/init/init.py:316:18:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/register/register.py#L59'>src/latch_cli/services/register/register.py:59:36:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/db.py#L151'>lnbits/db.py:151:34:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/formats/excel.py#L420'>pandas/io/formats/excel.py:420:35:</a> RUF039 First argument to `re.search()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/formats/style_render.py#L2509'>pandas/io/formats/style_render.py:2509:28:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/formats/style_render.py#L2511'>pandas/io/formats/style_render.py:2511:28:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/formats/style_render.py#L2514'>pandas/io/formats/style_render.py:2514:32:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/formats/style_render.py#L2516'>pandas/io/formats/style_render.py:2516:32:</a> RUF039 First argument to `re.findall()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/tests/dtypes/test_inference.py#L462'>pandas/tests/dtypes/test_inference.py:462:44:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/tests/dtypes/test_inference.py#L473'>pandas/tests/dtypes/test_inference.py:473:43:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/tests/extension/test_arrow.py#L1788'>pandas/tests/extension/test_arrow.py:1788:25:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/tests/frame/methods/test_replace.py#L1362'>pandas/tests/frame/methods/test_replace.py:1362:28:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/tests/indexes/datetimes/test_date_range.py#L787'>pandas/tests/indexes/datetimes/test_date_range.py:787:29:</a> RUF039 First argument to `re.split()` is not raw string
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/eeea375c410e0f874f1bd09172e468ce6e68f365/tests/test_integration.py#L31'>tests/test_integration.py:31:21:</a> RUF039 First argument to `re.compile()` is not raw string
+ <a href='https://github.com/pypa/build/blob/eeea375c410e0f874f1bd09172e468ce6e68f365/tests/test_integration.py#L32'>tests/test_integration.py:32:21:</a> RUF039 First argument to `re.compile()` is not raw string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L11'>src/poetry/console/logging/formatters/builder_formatter.py:11:26:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L13'>src/poetry/console/logging/formatters/builder_formatter.py:13:26:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L15'>src/poetry/console/logging/formatters/builder_formatter.py:15:26:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/logging/formatters/builder_formatter.py#L18'>src/poetry/console/logging/formatters/builder_formatter.py:18:17:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py#L22'>src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py:22:13:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py#L23'>src/poetry/mixology/solutions/providers/python_requirement_solution_provider.py:23:13:</a> RUF039 First argument to `re.match()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/puzzle/provider.py#L736'>src/poetry/puzzle/provider.py:736:21:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/dependency_specification.py#L192'>src/poetry/utils/dependency_specification.py:192:13:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/config/test_config.py#L58'>tests/config/test_config.py:58:46:</a> RUF039 First argument to `re.sub()` is not raw string
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/conftest.py#L378'>tests/conftest.py:378:20:</a> RUF039 First argument to `re.compile()` is not raw string
... 6 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF039 | 199 | 199 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:975 on 2024-11-19 07:20_

What's the motivation for picking 051 over 039 or 049?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:82 on 2024-11-19 07:21_

```suggestion
        f.write_str(match self {
            RegexModule::Re => "re",
            RegexModule::Regex => "regex",
        })
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:60 on 2024-11-19 07:22_

```suggestion
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:85 on 2024-11-19 07:22_

```suggestion
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:119 on 2024-11-19 07:24_

Nit: I find it more consistent if the matching logic lives in `RegexModule` considering that `RegexModule` already implements `Display`

```
impl FromStr for RegexModule { ... }
```

```suggestion
        let module = RegexModule::try_from(*module).ok()?;
        (module, *func)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:127 on 2024-11-19 07:25_

```suggestion
fn regex_module_and_func<'model>(semantic: &SemanticModel<'model>, expr: &Expr) -> Option<(RegexModule, &'model str)> {
    let qualified_name = semantic.resolve_qualified_name(expr)?;

    let (module, func) = match qualified_name.segments() {
        [module, func] => match *module {
            "re" => (RegexModule::Re, *func),
            "regex" => (RegexModule::Regex, *func),
            _ => return None,
        },
        _ => return None,
    };

    if is_shared(func) || module.is_regex() && is_regex_specific(func) {
        return Some((module, func));
    }

    None
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:125 on 2024-11-19 07:28_

Nit: I would make this a method on `module`

```suggestion
	module.is_regex_function(func)
```

and it can be implemented as

```rust
fn is_regex_function(name: &str) -> bool {
	matches!(
			name,  
			"compile"
            | "findall"
            | "finditer"
            | "fullmatch"
            | "match"
            | "search"
            | "split"
            | "sub"
            | "subn"
		) || (self == RegexModule::Regex && matches!(func, "splititer" | "subf" | "subfn" | "template")
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:138 on 2024-11-19 07:31_

What's the reason for excluding implicit concatenated strings? 

---

_@MichaReiser reviewed on 2024-11-19 07:33_

Thanks. This overall looks good. I'm interested in your and @AlexWaygood's opinion on flagging non-raw strings for all regex patterns or if we should make the rule slightly more clever, e.g. by allowing regex patterns containing an escape sequence that can't be written as a raw string (e.g. `\n`)

---

_Label `rule` added by @MichaReiser on 2024-11-19 07:33_

---

_Label `preview` added by @MichaReiser on 2024-11-19 07:33_

---

_@InSyncWithFoo reviewed on 2024-11-19 07:46_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/codes.rs`:975 on 2024-11-19 07:46_

I picked the number a few days ago, when there were a lot of opening PRs for `RUF` rules. I also made sure not to pick two consecutive numbers in case someone sees one of my PR and pick the next number in line. If I'm not mistaken, I have been submitting PRs for `RUF045`, `RUF048`, `RUF051` and `RUF054`.

Changing the code should be trivial.

---

_@InSyncWithFoo reviewed on 2024-11-19 07:50_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:138 on 2024-11-19 07:50_

I think I was avoiding weird cases where adding the `r` prefix might trigger another rule:
```python
re.compile(
    f'(?P<{group}>' # Should this have the redundant and visually annoying `r`?
    r'\s+'          # If the segment above had `r`, should this have `f`?
    ')'             # What about this?
)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:138 on 2024-11-19 07:54_

I don't think the rule would trigger for the f-string because the rule doesn't handle f-strings. 

I've seen that using implicit concatenated strings are sometimes used in combination with regexs that need to use an escaped character, e.g. `\n`. But it would be in the rule's spirit to require  the `r` prefix before `')'` because it would also require the prefix if it weren't an implicit concatenated string.

---

_@MichaReiser reviewed on 2024-11-19 07:54_

---

_Comment by @InSyncWithFoo on 2024-11-19 10:15_

I think this should be a blanket enforcement.

```python
# It's too easy to use a normal string as the pattern...
re.compile('uv')       # `uv` anywhere (`uv`, `uvicorn`, `dhruv`, `juvenile`, etc.)

# ...then forget to switch to a raw string when the pattern changes.
re.compile('\buv\b')   # `uv` not within a word?
```

---

_Comment by @MichaReiser on 2024-11-19 10:59_

I don't disagree with this overall, but it does mean that it gives you false positives for e.g. 

https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/io.py#L223-L230

---

_Comment by @MichaReiser on 2024-11-19 11:04_

So maybe that's specific to escape sequences. Python regex support all other common escape sequences (with the exception of `\b`, which maps to word bounderies). It's, therefore, not required to use a raw string for `\n`.

---

_Comment by @MichaReiser on 2024-11-19 11:05_

Oh, `\U` escape sequences are now also supported. I then agree that it should always flag.

> Changed in version 3.3: The '\u' and '\U' escape sequences have been added.

> Changed in version 3.6: Unknown escapes consisting of '\' and an ASCII letter now are errors.

> Changed in version 3.8: The '\N{name}' escape sequence has been added. As in string literals, it expands to the named Unicode character (e.g. '\N{EM DASH}').


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:18 on 2024-11-19 11:06_

Nit
```suggestion
/// - For `regex` and `re`: `compile`, `findall`, `finditer`,
///   `fullmatch`, `match`, `search`, `split`, `sub`, `subn`.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1739 on 2024-11-19 11:09_

I don't think this new methods are still needed. Can we remove them again? 

I'm a bit hesitant because `is_raw` only makes sense for individual part. There's no concept of a raw-string literal. 

---

_@MichaReiser approved on 2024-11-19 11:09_

---

_@MichaReiser reviewed on 2024-11-19 11:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:975 on 2024-11-19 11:39_

I'm happy to merge this soon. That's why I suggest we use `RUF039` for it, to avoid "holes". First come, first serve

---

_@AlexWaygood reviewed on 2024-11-19 11:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/mod.rs`:34 on 2024-11-19 11:48_

`cooked_re_pattern`?

(that's a joke!)

---

_Comment by @AlexWaygood on 2024-11-19 12:00_

I haven't studied this in depth, but it overall seems reasonable to me, both in concept and implementation.

I wondered if we needed to take account of [this footgun](https://docs.python.org/3/faq/design.html#why-can-t-raw-strings-r-strings-end-with-a-backslash) to do with raw strings (they cannot end with an odd number of backslashes). But I think we _should_ be fine, since that documentation states:

> Raw strings were designed to ease creating input for processors (chiefly regular expression engines) that want to do their own backslash escape processing. Such processors consider an unmatched trailing backslash to be an error anyway, so raw strings disallow that. In return, they allow you to pass on the string quote character by escaping it with a backslash. These rules work well when r-strings are used for their intended purpose.

---

_@InSyncWithFoo reviewed on 2024-11-19 12:09_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/codes.rs`:975 on 2024-11-19 12:09_

All done.

---

_Renamed from "[`ruff`] `re` and `regex` calls with unraw string as first argument (`RUF051`)" to "[`ruff`] `re` and `regex` calls with unraw string as first argument (`RUF039`)" by @InSyncWithFoo on 2024-11-19 12:09_

---

_Merged by @MichaReiser on 2024-11-19 12:44_

---

_Closed by @MichaReiser on 2024-11-19 12:44_

---

_Branch deleted on 2024-11-20 03:46_

---

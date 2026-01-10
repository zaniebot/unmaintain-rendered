```yaml
number: 14332
title: "[`flake8-pyi`] Apply `redundant-numeric-union` to more type expressions (`PYI041`)"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
base: main
head: pyi041-redundant-numeric-union-false-negatives
created_at: 2024-11-14T08:07:15Z
updated_at: 2024-11-25T19:05:05Z
url: https://github.com/astral-sh/ruff/pull/14332
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Apply `redundant-numeric-union` to more type expressions (`PYI041`)

---

_Pull request opened by @sbrugman on 2024-11-14 08:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes false negatives for redundant numeric unions in other places than function arguments, eg. return types, type aliases

Splitting this change of from https://github.com/astral-sh/ruff/pull/14273 (will rebase once that is merged)
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test` and reviewed ecosystem results
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-11-14 08:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+102 -0 violations, +0 -0 fixes in 11 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/src/plasmapy/analysis/swept_langmuir/floating_potential.py#L221'>src/plasmapy/analysis/swept_langmuir/floating_potential.py:221:37:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/src/plasmapy/formulary/densities.py#L159'>src/plasmapy/formulary/densities.py:159:32:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/tests/plasma/test_grids.py#L261'>tests/plasma/test_grids.py:261:32:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/tests/plasma/test_grids.py#L302'>tests/plasma/test_grids.py:302:32:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/configuration.py#L1168'>airflow/configuration.py:1168:10:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/configuration.py#L69'>airflow/configuration.py:69:14:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/jobs/job.py#L63'>airflow/jobs/job.py:63:62:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/jobs/job.py#L65'>airflow/jobs/job.py:65:35:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/metrics/otel_logger.py#L53'>airflow/metrics/otel_logger.py:53:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/metrics/protocols.py#L26'>airflow/metrics/protocols.py:26:13:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/policies.py#L109'>airflow/policies.py:109:54:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/serialization/helpers.py#L28'>airflow/serialization/helpers.py:28:65:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/serialization/serde.py#L59'>airflow/serialization/serde.py:59:5:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/utils/log/colored_log.py#L60'>airflow/utils/log/colored_log.py:60:33:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L201'>providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:201:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L301'>providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:301:54:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/operators/analyticdb_spark.py#L183'>providers/src/airflow/providers/alibaba/cloud/operators/analyticdb_spark.py:183:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/amazon/aws/hooks/glue.py#L111'>providers/src/airflow/providers/amazon/aws/hooks/glue.py:111:31:</a> PYI041 Use `float` instead of `int | float`
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/connectors/sqla/models.py#L1673'>superset/connectors/sqla/models.py:1673:10:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/db_engine_specs/base.py#L875'>superset/db_engine_specs/base.py:875:10:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/distributed_lock/utils.py#L31'>superset/distributed_lock/utils.py:31:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/models/helpers.py#L900'>superset/models/helpers.py:900:10:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/superset_typing.py#L105'>superset/superset_typing.py:105:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/core.py#L349'>superset/utils/core.py:349:53:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:20:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:46:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:65:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/resample.py#L30'>superset/utils/pandas_postprocessing/resample.py:30:26:</a> PYI041 Use `float` instead of `int | float`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L53'>src/bokeh/core/property/numeric.py:53:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/compare.py#L50'>tests/support/util/compare.py:50:18:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/registry/types.py#L37'>src/latch/registry/types.py:37:43:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/types/json.py#L11'>src/latch/types/json.py:11:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/types/metadata.py#L418'>src/latch/types/metadata.py:418:28:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_cli/snakemake/config/utils.py#L12'>src/latch_cli/snakemake/config/utils.py:12:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_cli/snakemake/config/utils.py#L196'>src/latch_cli/snakemake/config/utils.py:196:50:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_sdk_gql/__init__.py#L14'>src/latch_sdk_gql/__init__.py:14:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_sdk_gql/utils.py#L37'>src/latch_sdk_gql/utils.py:37:28:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/_sqlite3.pyi#L28'>stdlib/_sqlite3.pyi:28:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/ast.pyi#L1076'>stdlib/ast.pyi:1076:12:</a> PYI041 Use `complex` instead of `int | float | complex`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/ast.pyi#L1694'>stdlib/ast.pyi:1694:16:</a> PYI041 Use `complex` instead of `int | float | complex`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/dbm/sqlite3.pyi#L13'>stdlib/dbm/sqlite3.pyi:13:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/fractions.pyi#L8'>stdlib/fractions.pyi:8:29:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/marshal.pyi#L12'>stdlib/marshal.pyi:12:5:</a> PYI041 Use `complex` instead of `int | float | complex`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/pstats.pyi#L19'>stdlib/pstats.pyi:19:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/sqlite3/__init__.pyi#L216'>stdlib/sqlite3/__init__.pyi:216:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/tarfile.pyi#L531'>stdlib/tarfile.pyi:531:12:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/xmlrpc/client.pyi#L17'>stdlib/xmlrpc/client.pyi:17:5:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/assertpy/assertpy/numeric.pyi#L6'>stubs/assertpy/assertpy/numeric.pyi:6:23:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/docker/docker/_types.pyi#L10'>stubs/docker/docker/_types.pyi:10:19:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/fpdf2/fpdf/drawing.pyi#L19'>stubs/fpdf2/fpdf/drawing.pyi:19:21:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/gdb/gdb/__init__.pyi#L74'>stubs/gdb/gdb/__init__.pyi:74:29:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/gevent/gevent/timeout.pyi#L16'>stubs/gevent/gevent/timeout.pyi:16:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/hdbcli/hdbcli/dbapi.pyi#L135'>stubs/hdbcli/hdbcli/dbapi.pyi:135:14:</a> PYI041 Use `complex` instead of `int | float | complex`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/edfa5ca1c1c533fc735f27bd89499bcf823cac15/rotkehlchen/fval.py#L8'>rotkehlchen/fval.py:8:27:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/lib/data_types.py#L116'>zerver/lib/data_types.py:116:28:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/lib/drafts.py#L33'>zerver/lib/drafts.py:33:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/openapi/markdown_extension.py#L257'>zerver/openapi/markdown_extension.py:257:64:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zilencer/views.py#L588'>zilencer/views.py:588:16:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_channel.py#L114'>src/trio/_channel.py:114:22:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_channel.py#L123'>src/trio/_channel.py:123:22:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_sync.py#L159'>src/trio/_sync.py:159:19:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_sync.py#L229'>src/trio/_sync.py:229:28:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_sync.py#L236'>src/trio/_sync.py:236:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python-trio/trio/blob/7d9c4a6488dd61b3f3502529d00a6eda8e8b7dff/src/trio/_sync.py#L270'>src/trio/_sync.py:270:35:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/acf130311806cb1a627ea0e21369c7d6f586663a/src/_pytest/mark/structures.py#L472'>src/_pytest/mark/structures.py:472:27:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L75'>astropy/units/typing.py:75:19:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L78'>astropy/units/typing.py:78:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L79'>astropy/units/typing.py:79:24:</a> PYI041 Use `complex` instead of `int | float | complex`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 102 | 102 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+66 -0 violations, +0 -0 fixes in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/src/plasmapy/analysis/swept_langmuir/floating_potential.py#L221'>src/plasmapy/analysis/swept_langmuir/floating_potential.py:221:37:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/src/plasmapy/formulary/densities.py#L159'>src/plasmapy/formulary/densities.py:159:32:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/tests/plasma/test_grids.py#L261'>tests/plasma/test_grids.py:261:32:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/624461f00bb871ca7cb11ca02de730d2a41de5f0/tests/plasma/test_grids.py#L302'>tests/plasma/test_grids.py:302:32:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/configuration.py#L1168'>airflow/configuration.py:1168:10:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/configuration.py#L69'>airflow/configuration.py:69:14:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/jobs/job.py#L63'>airflow/jobs/job.py:63:62:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/jobs/job.py#L65'>airflow/jobs/job.py:65:35:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/metrics/otel_logger.py#L53'>airflow/metrics/otel_logger.py:53:15:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/metrics/protocols.py#L26'>airflow/metrics/protocols.py:26:13:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/policies.py#L109'>airflow/policies.py:109:54:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/serialization/helpers.py#L28'>airflow/serialization/helpers.py:28:65:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/serialization/serde.py#L59'>airflow/serialization/serde.py:59:5:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/airflow/utils/log/colored_log.py#L60'>airflow/utils/log/colored_log.py:60:33:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L201'>providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:201:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L301'>providers/src/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:301:54:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/alibaba/cloud/operators/analyticdb_spark.py#L183'>providers/src/airflow/providers/alibaba/cloud/operators/analyticdb_spark.py:183:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/amazon/aws/hooks/glue.py#L111'>providers/src/airflow/providers/amazon/aws/hooks/glue.py:111:31:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/apache/livy/hooks/livy.py#L340'>providers/src/airflow/providers/apache/livy/hooks/livy.py:340:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/apache/livy/hooks/livy.py#L429'>providers/src/airflow/providers/apache/livy/hooks/livy.py:429:54:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/apache/livy/hooks/livy.py#L725'>providers/src/airflow/providers/apache/livy/hooks/livy.py:725:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/apache/livy/hooks/livy.py#L810'>providers/src/airflow/providers/apache/livy/hooks/livy.py:810:54:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/apache/livy/operators/livy.py#L77'>providers/src/airflow/providers/apache/livy/operators/livy.py:77:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L1043'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:1043:20:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L1431'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:1431:20:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/2d9e7a51bb30658405e258fdf9e4cfece49b2b71/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L1815'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:1815:20:</a> PYI041 [*] Use `float` instead of `int | float`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/connectors/sqla/models.py#L1673'>superset/connectors/sqla/models.py:1673:10:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/db_engine_specs/base.py#L875'>superset/db_engine_specs/base.py:875:10:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/distributed_lock/utils.py#L31'>superset/distributed_lock/utils.py:31:15:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/models/helpers.py#L900'>superset/models/helpers.py:900:10:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/superset_typing.py#L105'>superset/superset_typing.py:105:15:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/core.py#L349'>superset/utils/core.py:349:53:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:20:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:46:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/boxplot.py#L34'>superset/utils/pandas_postprocessing/boxplot.py:34:65:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/0560c2615d5925fe1785924d88771e187065d908/superset/utils/pandas_postprocessing/resample.py#L30'>superset/utils/pandas_postprocessing/resample.py:30:26:</a> PYI041 [*] Use `float` instead of `int | float`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L53'>src/bokeh/core/property/numeric.py:53:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/compare.py#L50'>tests/support/util/compare.py:50:18:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/registry/types.py#L37'>src/latch/registry/types.py:37:43:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/types/json.py#L11'>src/latch/types/json.py:11:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch/types/metadata.py#L418'>src/latch/types/metadata.py:418:28:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_cli/snakemake/config/utils.py#L12'>src/latch_cli/snakemake/config/utils.py:12:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_cli/snakemake/config/utils.py#L196'>src/latch_cli/snakemake/config/utils.py:196:50:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_sdk_gql/__init__.py#L14'>src/latch_sdk_gql/__init__.py:14:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/latchbio/latch/blob/de64e70e4aa25ffa3f8c01a672d638c3878ab9f7/src/latch_sdk_gql/utils.py#L37'>src/latch_sdk_gql/utils.py:37:28:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/marshal.pyi#L12'>stdlib/marshal.pyi:12:5:</a> PYI041 Use `complex` instead of `int | float | complex`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stdlib/xmlrpc/client.pyi#L17'>stdlib/xmlrpc/client.pyi:17:5:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/69e3eb8be69430e7e84fcc38fb450cc87655c15b/stubs/seaborn/seaborn/utils.pyi#L43'>stubs/seaborn/seaborn/utils.pyi:43:5:</a> PYI041 Use `complex` instead of `float | complex`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/edfa5ca1c1c533fc735f27bd89499bcf823cac15/rotkehlchen/fval.py#L8'>rotkehlchen/fval.py:8:27:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/lib/data_types.py#L116'>zerver/lib/data_types.py:116:28:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/lib/drafts.py#L33'>zerver/lib/drafts.py:33:16:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zerver/openapi/markdown_extension.py#L257'>zerver/openapi/markdown_extension.py:257:64:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/zulip/zulip/blob/95bc5f85976b229a491ace27ccc167a1b65df022/zilencer/views.py#L588'>zilencer/views.py:588:16:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/acf130311806cb1a627ea0e21369c7d6f586663a/src/_pytest/mark/structures.py#L472'>src/_pytest/mark/structures.py:472:27:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L75'>astropy/units/typing.py:75:19:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L78'>astropy/units/typing.py:78:24:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/typing.py#L79'>astropy/units/typing.py:79:24:</a> PYI041 [*] Use `complex` instead of `int | float | complex`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 66 | 66 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-11-14 18:38_

Note for myself, merge https://github.com/astral-sh/ruff/pull/14273 after this PR lands

---

_Marked ready for review by @sbrugman on 2024-11-25 18:33_

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-25 18:33_

---

_@AlexWaygood requested changes on 2024-11-25 18:39_

Thanks for the PR. Unfortunately, I think the premise of this PR is incorrect. As the docs for this rule state:

```
/// ## Why is this bad?
/// The [typing specification] states:
///
/// > Python’s numeric types `complex`, `float` and `int` are not subtypes of
/// > each other, but to support common use cases, the type system contains a
/// > straightforward shortcut: when an argument is annotated as having type
/// > `float`, an argument of type `int` is acceptable; similar, for an
/// > argument annotated as having type `complex`, arguments of type `float` or
/// > `int` are acceptable.
///
/// As such, a union that includes both `int` and `float` is redundant in the
/// specific context of a parameter annotation, as it is equivalent to a union
/// that only includes `float`. For readability and clarity, unions should omit
/// redundant elements.
```

Note the specific language of the typing spec there: `float` is _not_ a subtype of `complex`, and `int` is _not_ a subtype of `float`. You cannot _always_ simplify the union `int | float` to `float`, you can _only_ do this "in the specific context of a parameter annotation", because of a very special case that the typing system carves out for this situation

---

_Comment by @sbrugman on 2024-11-25 19:05_

Ah, thanks for pointing that out! I wrongly assumed the scope to parameters was unintentional and that this rule applied in all type annotation contexts.

---

_Closed by @sbrugman on 2024-11-25 19:05_

---

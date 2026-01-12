```yaml
number: 18618
title: "[`pandas-vet`] Deprecate `pandas-df-variable-name` (`PD901`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - breaking
assignees: []
merged: true
base: brent/release-0.12.0
head: brent/deprecate-pd901
created_at: 2025-06-10T20:36:55Z
updated_at: 2025-06-12T14:26:17Z
url: https://github.com/astral-sh/ruff/pull/18618
synced_at: 2026-01-12T15:56:22Z
```

# [`pandas-vet`] Deprecate `pandas-df-variable-name` (`PD901`)

---

_@ntBre_

Summary
--

Deprecates PD901 as part of #7710. I don't feel particularly strongly about this one, though I have certainly used `df` as a dataframe name in the past, just going through the open issues in the 0.12 milestone.

Test Plan
--

N/a


---

_Added to milestone `v0.12` by @ntBre on 2025-06-10 20:36_

---

_Label `rule` added by @ntBre on 2025-06-10 20:36_

---

_Label `breaking` added by @ntBre on 2025-06-10 20:36_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 20:44_

---

_Comment by @github-actions[bot] on 2025-06-10 20:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -430 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -72 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/airflow-core/src/airflow/example_dags/tutorial_objectstorage.py#L93'>airflow-core/src/airflow/example_dags/tutorial_objectstorage.py:93:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/airflow-core/src/airflow/serialization/serializers/pandas.py#L68'>airflow-core/src/airflow/serialization/serializers/pandas.py:68:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/dev/airflow_perf/sql_queries.py#L186'>dev/airflow_perf/sql_queries.py:186:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- dev/stats/explore_pr_candidates.ipynb:cell 3:23:5: PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L232'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:232:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L283'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:283:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L326'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:326:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py#L116'>providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py:116:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py#L99'>providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py:99:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/druid/tests/unit/apache/druid/hooks/test_druid.py#L461'>providers/apache/druid/tests/unit/apache/druid/hooks/test_druid.py:461:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -267 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L110'>superset/charts/client_processing.py:110:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L112'>superset/charts/client_processing.py:112:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L113'>superset/charts/client_processing.py:113:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L114'>superset/charts/client_processing.py:114:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L124'>superset/charts/client_processing.py:124:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L132'>superset/charts/client_processing.py:132:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L136'>superset/charts/client_processing.py:136:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L141'>superset/charts/client_processing.py:141:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L144'>superset/charts/client_processing.py:144:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L147'>superset/charts/client_processing.py:147:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L206'>superset/charts/client_processing.py:206:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L213'>superset/charts/client_processing.py:213:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L341'>superset/charts/client_processing.py:341:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L343'>superset/charts/client_processing.py:343:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L91'>superset/charts/client_processing.py:91:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L95'>superset/charts/client_processing.py:95:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/database/uploaders/csv_reader.py#L135'>superset/commands/database/uploaders/csv_reader.py:135:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/database/uploaders/excel_reader.py#L108'>superset/commands/database/uploaders/excel_reader.py:108:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/dataset/importers/v1/utils.py#L207'>superset/commands/dataset/importers/v1/utils.py:207:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/report/alert.py#L179'>superset/commands/report/alert.py:179:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/report/alert.py#L204'>superset/commands/report/alert.py:204:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/sql_lab/export.py#L104'>superset/commands/sql_lab/export.py:104:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/sql_lab/export.py#L128'>superset/commands/sql_lab/export.py:128:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_actions.py#L104'>superset/common/query_actions.py:104:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L268'>superset/common/query_context_processor.py:268:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L275'>superset/common/query_context_processor.py:275:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L279'>superset/common/query_context_processor.py:279:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L287'>superset/common/query_context_processor.py:287:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L602'>superset/common/query_context_processor.py:602:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L658'>superset/common/query_context_processor.py:658:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L665'>superset/common/query_context_processor.py:665:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 236 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -91 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD901 | 430 | 0 | 430 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -430 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -72 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/airflow-core/src/airflow/example_dags/tutorial_objectstorage.py#L93'>airflow-core/src/airflow/example_dags/tutorial_objectstorage.py:93:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/airflow-core/src/airflow/serialization/serializers/pandas.py#L68'>airflow-core/src/airflow/serialization/serializers/pandas.py:68:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/dev/airflow_perf/sql_queries.py#L186'>dev/airflow_perf/sql_queries.py:186:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- dev/stats/explore_pr_candidates.ipynb:cell 3:23:5: PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L232'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:232:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L283'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:283:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py#L326'>providers/amazon/tests/unit/amazon/aws/transfers/test_sql_to_s3.py:326:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py#L116'>providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py:116:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py#L99'>providers/apache/drill/tests/unit/apache/drill/hooks/test_drill.py:99:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a3257f766086d79144680e90f714c043bfe4c70/providers/apache/druid/tests/unit/apache/druid/hooks/test_druid.py#L461'>providers/apache/druid/tests/unit/apache/druid/hooks/test_druid.py:461:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -267 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L110'>superset/charts/client_processing.py:110:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L112'>superset/charts/client_processing.py:112:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L113'>superset/charts/client_processing.py:113:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L114'>superset/charts/client_processing.py:114:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L124'>superset/charts/client_processing.py:124:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L132'>superset/charts/client_processing.py:132:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L136'>superset/charts/client_processing.py:136:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L141'>superset/charts/client_processing.py:141:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L144'>superset/charts/client_processing.py:144:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L147'>superset/charts/client_processing.py:147:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L206'>superset/charts/client_processing.py:206:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L213'>superset/charts/client_processing.py:213:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L341'>superset/charts/client_processing.py:341:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L343'>superset/charts/client_processing.py:343:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L91'>superset/charts/client_processing.py:91:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/charts/client_processing.py#L95'>superset/charts/client_processing.py:95:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/database/uploaders/csv_reader.py#L135'>superset/commands/database/uploaders/csv_reader.py:135:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/database/uploaders/excel_reader.py#L108'>superset/commands/database/uploaders/excel_reader.py:108:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/dataset/importers/v1/utils.py#L207'>superset/commands/dataset/importers/v1/utils.py:207:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/report/alert.py#L179'>superset/commands/report/alert.py:179:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/report/alert.py#L204'>superset/commands/report/alert.py:204:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/sql_lab/export.py#L104'>superset/commands/sql_lab/export.py:104:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/commands/sql_lab/export.py#L128'>superset/commands/sql_lab/export.py:128:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_actions.py#L104'>superset/common/query_actions.py:104:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L268'>superset/common/query_context_processor.py:268:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L275'>superset/common/query_context_processor.py:275:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L279'>superset/common/query_context_processor.py:279:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L287'>superset/common/query_context_processor.py:287:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L602'>superset/common/query_context_processor.py:602:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L658'>superset/common/query_context_processor.py:658:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/common/query_context_processor.py#L665'>superset/common/query_context_processor.py:665:17:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 236 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -91 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD901 | 430 | 0 | 430 | 0 | 0 |

</p>
</details>




---

_Review request for @AlexWaygood removed by @ntBre on 2025-06-10 20:49_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-10 20:49_

---

_@MichaReiser approved on 2025-06-12 07:38_

I share your sentiment. I wouldn't add this rule to Ruff today but I also think it isn't harmful. 

Either way, I think this is heading in the right direction and we could consider adding a new lint rule instead that allows users to configure a set of names that they want to disallow.

---

_@dylwil3 approved on 2025-06-12 14:21_

---

_Merged by @ntBre on 2025-06-12 14:26_

---

_Closed by @ntBre on 2025-06-12 14:26_

---

_Branch deleted on 2025-06-12 14:26_

---

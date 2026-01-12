```yaml
number: 5485
title: "Move `pandas-df-variable-name` (`PD901`) to the nursery"
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
base: main
head: charlie/pd
created_at: 2023-07-03T17:28:58Z
updated_at: 2023-07-03T18:00:10Z
url: https://github.com/astral-sh/ruff/pull/5485
synced_at: 2026-01-12T15:55:18Z
```

# Move `pandas-df-variable-name` (`PD901`) to the nursery

---

_@charliermarsh_

Closes #3330.

---

_Label `rule` added by @charliermarsh on 2023-07-03 17:29_

---

_Closed by @charliermarsh on 2023-07-03 17:29_

---

_Branch deleted on 2023-07-03 17:29_

---

_Comment by @github-actions[bot] on 2023-07-03 17:51_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -135, 0 error(s))

<details><summary>airflow (+0, -47)</summary>
<p>

```diff
- airflow/providers/apache/hive/hooks/hive.py:1057:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/exasol/hooks/exasol.py:80:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/presto/hooks/presto.py:167:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/presto/hooks/presto.py:170:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/salesforce/hooks/salesforce.py:304:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/salesforce/hooks/salesforce.py:361:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/slack/transfers/sql_to_slack.py:184:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/slack/transfers/sql_to_slack.py:77:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/trino/hooks/trino.py:187:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/providers/trino/hooks/trino.py:190:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- airflow/serialization/serializers/pandas.py:68:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- dev/perf/sql_queries.py:183:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_branch_python.py:47:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:101:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:127:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:153:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:177:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:49:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/decorators/test_sensor.py:77:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/amazon/aws/transfers/test_sql_to_s3.py:205:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/amazon/aws/transfers/test_sql_to_s3.py:256:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/drill/hooks/test_drill.py:100:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/druid/hooks/test_druid.py:235:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:266:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:314:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:597:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:726:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:742:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/hive/hooks/test_hive.py:787:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/impala/hooks/test_impala.py:86:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/apache/pinot/hooks/test_pinot.py:270:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/elasticsearch/hooks/test_elasticsearch.py:112:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/hooks/test_bigquery_system.py:37:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/hooks/test_bigquery_system.py:46:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/hooks/test_bigquery_system.py:50:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:405:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:427:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:449:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:479:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:508:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:533:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:583:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/presto/hooks/test_presto.py:270:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/slack/transfers/test_sql_to_slack.py:104:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/sqlite/hooks/test_sqlite.py:107:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/trino/hooks/test_trino.py:310:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/providers/vertica/hooks/test_vertica.py:177:9: PD901 `df` is a bad variable name. Be kinder to your future self.
```

</p>
</details>
<details><summary>bokeh (+0, -88)</summary>
<p>

```diff
- examples/basic/annotations/band.py:21:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/basic/annotations/band.py:25:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/basic/annotations/polygon_annotation.py:12:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/basic/areas/stacked_area.py:16:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/basic/axes/datetime_axis.py:6:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/basic/bars/intervals.py:14:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/interaction/legends/legend_hide.py:22:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/interaction/legends/legend_mute.py:11:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/models/daylight.py:24:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/models/trail.py:36:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/brewer.py:19:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/interactive_legend.py:23:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/periodic_shells.py:27:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/periodic_shells.py:31:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/periodic_shells.py:32:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/plotting/periodic_shells.py:33:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/multi_select_server.py:13:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/radio_button_group_server.py:13:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/radio_group_server.py:13:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/range_slider_server.py:9:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/select_server.py:13:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/reference/models/slider_server.py:13:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/api/flask_embed.py:18:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/api/flask_gunicorn_embed.py:33:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/api/standalone_embed.py:10:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/api/tornado_embed.py:21:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/crossfilter/main.py:19:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/export_csv/main.py:15:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/gapminder/main.py:18:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/movies/main.py:91:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/stocks/app_hooks.py:10:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/weather/main.py:25:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/weather/main.py:31:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/server/app/weather/main.py:88:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/styling/plots/legend_title.py:10:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/categorical/heatmap_unemployment.py:29:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/categorical/periodic.py:21:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/categorical/periodic.py:25:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/categorical/periodic.py:26:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/categorical/periodic.py:27:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/geo/eclipse.py:31:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/hierarchical/treemap.py:39:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/stats/boxplot.py:20:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/stats/boxplot.py:28:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/stats/splom.py:24:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/timeseries/candlestick.py:19:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- examples/topics/timeseries/missing_dates.py:17:1: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/models/util/structure.py:218:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/models/util/structure.py:309:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/models/util/structure.py:317:13: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/autompg.py:74:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/autompg2.py:73:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/browsers.py:74:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/daylight.py:69:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/population.py:67:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/population.py:68:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/population.py:69:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/population.py:70:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/sea_surface_temperature.py:66:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sampledata/sea_surface_temperature.py:67:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/sphinxext/bokeh_dataframe.py:84:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- src/bokeh/util/hex.py:204:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/core/property/test_container.py:283:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/document/test_events__document.py:319:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_mappers.py:80:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:105:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:117:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:129:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:144:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:157:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:171:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:177:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:220:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:232:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:308:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:312:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:324:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:331:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:401:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:425:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:449:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:79:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/models/test_sources.py:91:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/plotting/test_figure.py:236:9: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/util/test_util__serialization.py:162:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/util/test_util__serialization.py:166:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/util/test_util__serialization.py:170:5: PD901 `df` is a bad variable name. Be kinder to your future self.
- tests/unit/bokeh/util/test_util__serialization.py:174:5: PD901 `df` is a bad variable name. Be kinder to your future self.
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PD901 | 135 | 0 | 135 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.00      9.3±0.01ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     8.1 MB/sec    1.00      2.1±0.00ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    229.8±0.44µs    12.8 MB/sec    1.01    231.2±1.46µs    12.8 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.01ms     5.7 MB/sec    1.00      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.05ms     2.5 MB/sec    1.00     16.0±0.05ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.01      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.6±0.74µs     5.8 MB/sec    1.03    528.5±2.98µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.01ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.02      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1738.9±3.44µs     9.6 MB/sec    1.02   1778.3±4.69µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.4±0.32µs    15.0 MB/sec    1.03    201.5±0.73µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.02      3.7±0.00ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.01      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1951.5±10.91µs     8.5 MB/sec    1.01   1967.3±8.02µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    215.3±3.04µs    13.7 MB/sec    1.01    217.7±6.11µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.8 MB/sec    1.01      4.4±0.04ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.06ms     2.6 MB/sec    1.00     15.4±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.3±4.83µs     6.9 MB/sec    1.00    426.2±5.82µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.01      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1674.3±10.87µs     9.9 MB/sec    1.00   1668.1±9.68µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.9±1.16µs    16.2 MB/sec    1.00    180.2±0.81µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

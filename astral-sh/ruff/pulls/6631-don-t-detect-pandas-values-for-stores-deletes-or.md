```yaml
number: 6631
title: "Don't detect `pandas#values` for stores, deletes, or class accesses"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pd
created_at: 2023-08-16T20:56:06Z
updated_at: 2023-08-16T21:22:52Z
url: https://github.com/astral-sh/ruff/pull/6631
synced_at: 2026-01-12T15:55:22Z
```

# Don't detect `pandas#values` for stores, deletes, or class accesses

---

_@charliermarsh_

## Summary

Ensures we avoid cases like:

```python
x.values = 1
```

Since Pandas doesn't even expose a setter for that. We also avoid cases like:

```python
print(self.values)
```

Since it's overwhelming likely to be a false positive.

Closes https://github.com/astral-sh/ruff/issues/6630.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-16 20:56_

---

_Marked ready for review by @charliermarsh on 2023-08-16 20:56_

---

_@zanieb approved on 2023-08-16 21:01_

---

_Comment by @github-actions[bot] on 2023-08-16 21:07_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -23, 0 error(s))

<details><summary>airflow (+0, -7)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L45'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:45:9:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/models/xcom_arg.py#L527'>airflow/models/xcom_arg.py:527:9:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/models/xcom_arg.py#L540'>airflow/models/xcom_arg.py:540:83:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/models/xcom_arg.py#L543'>airflow/models/xcom_arg.py:543:36:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/providers/google/cloud/operators/translate.py#L108'>airflow/providers/google/cloud/operators/translate.py:108:9:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/airflow/providers/google/leveldb/operators/leveldb.py#L67'>airflow/providers/google/leveldb/operators/leveldb.py:67:9:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/bfe08a79db8130c499883f014121be570ec071bd/tests/providers/google/suite/transfers/test_sql_to_sheets.py#L38'>tests/providers/google/suite/transfers/test_sql_to_sheets.py:38:9:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary>bokeh (+0, -16)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L126'>tests/integration/widgets/tables/test_cell_editors.py:126:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L135'>tests/integration/widgets/tables/test_cell_editors.py:135:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L148'>tests/integration/widgets/tables/test_cell_editors.py:148:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L159'>tests/integration/widgets/tables/test_cell_editors.py:159:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L171'>tests/integration/widgets/tables/test_cell_editors.py:171:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L200'>tests/integration/widgets/tables/test_cell_editors.py:200:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L209'>tests/integration/widgets/tables/test_cell_editors.py:209:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L222'>tests/integration/widgets/tables/test_cell_editors.py:222:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L233'>tests/integration/widgets/tables/test_cell_editors.py:233:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L245'>tests/integration/widgets/tables/test_cell_editors.py:245:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L273'>tests/integration/widgets/tables/test_cell_editors.py:273:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L282'>tests/integration/widgets/tables/test_cell_editors.py:282:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L317'>tests/integration/widgets/tables/test_cell_editors.py:317:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_cell_editors.py#L338'>tests/integration/widgets/tables/test_cell_editors.py:338:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/integration/widgets/tables/test_data_table.py#L43'>tests/integration/widgets/tables/test_data_table.py:43:46:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/a703abeeadad05342b2de34870ccaa937c233bc3/tests/unit/bokeh/core/test_serialization.py#L805'>tests/unit/bokeh/core/test_serialization.py:805:17:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PD011 | 23 | 0 | 23 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.4±0.17ms     9.2 MB/sec    1.00      4.4±0.17ms     9.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   891.4±29.57µs    18.7 MB/sec    1.02   905.0±30.78µs    18.4 MB/sec
formatter/numpy/globals.py                 1.00    89.8±17.00µs    32.8 MB/sec    1.02     91.3±4.92µs    32.3 MB/sec
formatter/pydantic/types.py                1.00  1759.1±59.69µs    14.5 MB/sec    1.04  1831.9±85.08µs    13.9 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.46ms     2.6 MB/sec    1.00     15.3±0.40ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.0±0.31ms     4.1 MB/sec    1.00      3.9±0.16ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.1±20.36µs     5.6 MB/sec    1.00   532.0±21.96µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.31ms     3.2 MB/sec    1.00      8.0±0.29ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.27ms     5.1 MB/sec    1.03      8.2±0.22ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1602.5±46.09µs    10.4 MB/sec    1.00  1609.4±41.73µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   200.4±16.70µs    14.7 MB/sec    1.00   197.5±10.97µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.7±0.42ms     6.9 MB/sec    1.00      3.5±0.09ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.8±0.23ms     8.4 MB/sec    1.03      5.0±0.24ms     8.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   931.8±49.59µs    17.9 MB/sec    1.07   998.5±37.68µs    16.7 MB/sec
formatter/numpy/globals.py                 1.00     93.9±4.50µs    31.4 MB/sec    1.12    104.8±8.65µs    28.2 MB/sec
formatter/pydantic/types.py                1.00  1911.2±91.34µs    13.3 MB/sec    1.08      2.1±0.12ms    12.4 MB/sec
linter/all-rules/large/dataset.py          1.04     15.3±0.43ms     2.7 MB/sec    1.00     14.8±0.41ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.17ms     4.1 MB/sec    1.02      4.1±0.23ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   512.1±29.94µs     5.8 MB/sec    1.05   540.1±20.53µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.28ms     3.3 MB/sec    1.02      7.9±0.28ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.23ms     5.0 MB/sec    1.08      8.7±0.26ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1635.8±80.36µs    10.2 MB/sec    1.10  1799.8±45.64µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.4±15.61µs    14.0 MB/sec    1.01   213.3±12.21µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.12ms     6.9 MB/sec    1.05      3.9±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-16 21:13_

---

_Closed by @charliermarsh on 2023-08-16 21:13_

---

_Branch deleted on 2023-08-16 21:13_

---

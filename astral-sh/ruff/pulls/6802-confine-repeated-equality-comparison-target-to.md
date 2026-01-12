```yaml
number: 6802
title: Confine repeated-equality-comparison-target to names and attributes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/yoda
created_at: 2023-08-23T03:48:03Z
updated_at: 2023-08-23T04:15:27Z
url: https://github.com/astral-sh/ruff/pull/6802
synced_at: 2026-01-12T15:55:22Z
```

# Confine repeated-equality-comparison-target to names and attributes

---

_@charliermarsh_

Empirically, Pylint does this, so seems reasonable to follow.

---

_Label `bug` added by @charliermarsh on 2023-08-23 03:51_

---

_Merged by @charliermarsh on 2023-08-23 03:56_

---

_Closed by @charliermarsh on 2023-08-23 03:56_

---

_Branch deleted on 2023-08-23 03:56_

---

_Comment by @github-actions[bot] on 2023-08-23 04:00_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -19, 0 error(s))

<details><summary>airflow (+0, -14)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/common/sql/operators/sql.py#L973'>airflow/providers/common/sql/operators/sql.py:973:16:</a> PLR1714 Consider merging multiple comparisons: `0 in (cur, ref)`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/databricks/hooks/databricks_base.py#L498'>airflow/providers/databricks/hooks/databricks_base.py:498:16:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/databricks/hooks/databricks_base.py#L507'>airflow/providers/databricks/hooks/databricks_base.py:507:16:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/databricks/hooks/databricks_base.py#L526'>airflow/providers/databricks/hooks/databricks_base.py:526:16:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/databricks/hooks/databricks_base.py#L535'>airflow/providers/databricks/hooks/databricks_base.py:535:16:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/hooks/bigquery.py#L3281'>airflow/providers/google/cloud/hooks/bigquery.py:3281:16:</a> PLR1714 Consider merging multiple comparisons: `0 in (cur, ref)`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/operators/bigquery_dts.py#L395'>airflow/providers/google/cloud/operators/bigquery_dts.py:395:12:</a> PLR1714 Consider merging multiple comparisons: `event["status"] in ("failed", "cancelled")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/operators/dataflow.py#L709'>airflow/providers/google/cloud/operators/dataflow.py:709:12:</a> PLR1714 Consider merging multiple comparisons: `event["status"] in ("error", "stopped")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/operators/dataflow.py#L894'>airflow/providers/google/cloud/operators/dataflow.py:894:12:</a> PLR1714 Consider merging multiple comparisons: `event["status"] in ("error", "stopped")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/operators/dataproc.py#L1861'>airflow/providers/google/cloud/operators/dataproc.py:1861:12:</a> PLR1714 Consider merging multiple comparisons: `event["status"] in ("failed", "error")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/operators/dataproc.py#L1992'>airflow/providers/google/cloud/operators/dataproc.py:1992:12:</a> PLR1714 Consider merging multiple comparisons: `event["status"] in ("failed", "error")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/airflow/providers/google/cloud/triggers/bigquery.py#L358'>airflow/providers/google/cloud/triggers/bigquery.py:358:22:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/tests/providers/celery/executors/test_celery_executor.py#L179'>tests/providers/celery/executors/test_celery_executor.py:179:24:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/a54c2424df51bf1acec420f4792a237dabcfa12b/tests/providers/google/cloud/operators/test_bigquery.py#L1970'>tests/providers/google/cloud/operators/test_bigquery.py:1970:16:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>bokeh (+0, -2)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/ed5f22bdc0e23a87436fac813090beb8a77f2d24/src/bokeh/io/export.py#L126'>src/bokeh/io/export.py:126:8:</a> PLR1714 Consider merging multiple comparisons: `0 in (image.width, image.height)`. Use a `set` if the elements are hashable.
- <a href='https://github.com/bokeh/bokeh/blob/ed5f22bdc0e23a87436fac813090beb8a77f2d24/src/bokeh/layouts.py#L450'>src/bokeh/layouts.py:450:20:</a> PLR1714 Consider merging multiple comparisons: `0 not in (child.nrows, child.ncols)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>zulip (+0, -3)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/3de007d9cd02ab30e82e00a821096b2946e7a4f7/zerver/forms.py#L96'>zerver/forms.py:96:8:</a> PLR1714 Consider merging multiple comparisons: `"-" in (subdomain[0], subdomain[-1])`. Use a `set` if the elements are hashable.
- <a href='https://github.com/zulip/zulip/blob/3de007d9cd02ab30e82e00a821096b2946e7a4f7/zerver/lib/domains.py#L15'>zerver/lib/domains.py:15:8:</a> PLR1714 Consider merging multiple comparisons: `"." in (domain[0], domain[-1])`. Use a `set` if the elements are hashable.
- <a href='https://github.com/zulip/zulip/blob/3de007d9cd02ab30e82e00a821096b2946e7a4f7/zerver/lib/domains.py#L20'>zerver/lib/domains.py:20:12:</a> PLR1714 Consider merging multiple comparisons: `"-" in (subdomain[0], subdomain[-1])`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR1714 | 19 | 0 | 19 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.02ms    12.1 MB/sec    1.00      3.4±0.01ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    714.6±2.47µs    23.3 MB/sec    1.00    714.6±4.84µs    23.3 MB/sec
formatter/numpy/globals.py                 1.00     76.8±0.29µs    38.4 MB/sec    1.00     76.8±0.30µs    38.4 MB/sec
formatter/pydantic/types.py                1.01  1402.1±21.51µs    18.2 MB/sec    1.00  1388.4±31.58µs    18.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.01ms     4.0 MB/sec    1.06     10.7±0.05ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.00ms     6.1 MB/sec    1.03      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.2±0.62µs     7.7 MB/sec    1.01    389.0±0.38µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.03ms     4.8 MB/sec    1.04      5.5±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.09      5.9±0.01ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1196.0±0.94µs    13.9 MB/sec    1.07   1274.4±3.87µs    13.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.5±0.27µs    21.5 MB/sec    1.04    143.1±0.80µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.07      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      3.7±0.05ms    11.0 MB/sec     1.01      3.7±0.06ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   758.1±11.85µs    22.0 MB/sec     1.01   762.1±18.20µs    21.8 MB/sec
formatter/numpy/globals.py                 1.00     79.0±2.69µs    37.4 MB/sec     1.02     80.6±3.01µs    36.6 MB/sec
formatter/pydantic/types.py                1.00  1537.1±57.70µs    16.6 MB/sec     1.00  1533.5±25.20µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.02     12.7±0.15ms     3.2 MB/sec     1.00     12.5±0.14ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.8 MB/sec     1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.5±6.35µs     6.8 MB/sec     1.00    429.8±5.03µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.11ms     3.8 MB/sec     1.00      6.5±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.12ms     5.8 MB/sec     1.01      7.1±0.11ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1543.1±146.71µs    10.8 MB/sec    1.00  1509.0±23.04µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.0±3.29µs    17.0 MB/sec     1.00    173.4±3.71µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.04ms     8.0 MB/sec     1.00      3.2±0.06ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

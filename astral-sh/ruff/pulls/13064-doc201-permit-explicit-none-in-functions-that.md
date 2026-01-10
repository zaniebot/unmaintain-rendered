```yaml
number: 13064
title: "[`DOC201`] Permit explicit `None` in functions that only return `None`"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
  - docstring
  - preview
assignees: []
merged: true
base: main
head: doc201-explicit-none
created_at: 2024-08-22T22:52:24Z
updated_at: 2024-08-27T16:33:05Z
url: https://github.com/astral-sh/ruff/pull/13064
synced_at: 2026-01-10T21:38:32Z
```

# [`DOC201`] Permit explicit `None` in functions that only return `None`

---

_Pull request opened by @tjkuson on 2024-08-22 22:52_

## Summary

No longer reports [docstring-missing-returns (DOC201)](https://docs.astral.sh/ruff/rules/docstring-missing-returns/) on explicit returns in functions that only return `None`.

For example,

```python
def foo(obj: object) -> None:
    """A very helpful docstring.

    Args:
        obj (object): An object.
    """
    if obj is None:
        return
    print(obj)
```

no longer reports DOC201.

Closes #13062

### Methodology

When visiting, count the number of `return None`s visited in the body. There are then two scenarios under which the diagnostic is skipped:

1. If the return is not annotated and if the number of `return None` to the total number of returns (that is to say, all the paths return `None`), skip the diagnostic.
2. If the return is annotated, and that annotation is `None`, skip the diagnostic.

#### Return annotations

I saw in the ecosystem check that functions that only return `None` but annotated otherwise were now being ignored. For example,

```python
def foo(s: str) -> str | None:
    """A very helpful docstring.

    Args:
        s (str): A string.
    """
    return None
```

would no longer report a diagnostic. I changed this back by always checking the rule if the return annotation is not `None`: if the return annotation is anything other than `None`, run the diagnostic.

I think this is more likely to be the expected behaviour, especially due to the functional overlap between docstrings and type annotations. You _could_ argue that checking against the return type annotation should be all this patch does, but that would make the rule quite unhelpful in untyped contexts. For example,

```python
def foo(obj):
    """A very helpful docstring.

    Args:
        obj (object): An object.
    """
    if obj is None:
        return None
    print(obj)

```

would still report DOC201.

## Test Plan

`cargo nextest run`


---

_Marked ready for review by @tjkuson on 2024-08-22 22:59_

---

_Renamed from "[`DOC201`] Permit explicit None early returns" to "[`DOC201`] Permit explicit None in functions that only return None" by @tjkuson on 2024-08-22 23:05_

---

_Renamed from "[`DOC201`] Permit explicit None in functions that only return None" to "[`DOC201`] Permit explicit `None` in functions that only return `None`" by @tjkuson on 2024-08-22 23:05_

---

_Comment by @github-actions[bot] on 2024-08-22 23:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -67 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -51 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/dag_processing/processor.py#L692'>airflow/dag_processing/processor.py:692:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/io/path.py#L386'>airflow/io/path.py:386:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/datadog_logger.py#L117'>airflow/metrics/datadog_logger.py:117:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/datadog_logger.py#L138'>airflow/metrics/datadog_logger.py:138:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/datadog_logger.py#L76'>airflow/metrics/datadog_logger.py:76:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/datadog_logger.py#L96'>airflow/metrics/datadog_logger.py:96:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/statsd_logger.py#L109'>airflow/metrics/statsd_logger.py:109:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/statsd_logger.py#L125'>airflow/metrics/statsd_logger.py:125:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/statsd_logger.py#L139'>airflow/metrics/statsd_logger.py:139:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/metrics/statsd_logger.py#L94'>airflow/metrics/statsd_logger.py:94:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/models/connection.py#L182'>airflow/models/connection.py:182:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/models/dag.py#L1270'>airflow/models/dag.py:1270:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/models/dag.py#L1275'>airflow/models/dag.py:1275:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/models/dagbag.py#L270'>airflow/models/dagbag.py:270:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/models/variable.py#L332'>airflow/models/variable.py:332:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/airbyte/operators/airbyte.py#L126'>airflow/providers/airbyte/operators/airbyte.py:126:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/airbyte/operators/airbyte.py#L139'>airflow/providers/airbyte/operators/airbyte.py:139:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/airbyte/sensors/airbyte.py#L162'>airflow/providers/airbyte/sensors/airbyte.py:162:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/amazon/aws/hooks/s3.py#L1327'>airflow/providers/amazon/aws/hooks/s3.py:1327:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/amazon/aws/sensors/s3.py#L407'>airflow/providers/amazon/aws/sensors/s3.py:407:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/apache/hive/hooks/hive.py#L431'>airflow/providers/apache/hive/hooks/hive.py:431:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/common/compat/openlineage/facet.py#L30'>airflow/providers/common/compat/openlineage/facet.py:30:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L1295'>airflow/providers/google/cloud/hooks/bigquery.py:1295:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L2600'>airflow/providers/google/cloud/hooks/bigquery.py:2600:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L2626'>airflow/providers/google/cloud/hooks/bigquery.py:2626:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L2639'>airflow/providers/google/cloud/hooks/bigquery.py:2639:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L2744'>airflow/providers/google/cloud/hooks/bigquery.py:2744:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery.py#L2796'>airflow/providers/google/cloud/hooks/bigquery.py:2796:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/bigquery_dts.py#L188'>airflow/providers/google/cloud/hooks/bigquery_dts.py:188:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/life_sciences.py#L164'>airflow/providers/google/cloud/hooks/life_sciences.py:164:17:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/hooks/secret_manager.py#L345'>airflow/providers/google/cloud/hooks/secret_manager.py:345:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataplex.py#L1027'>airflow/providers/google/cloud/operators/dataplex.py:1027:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataplex.py#L1207'>airflow/providers/google/cloud/operators/dataplex.py:1207:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataplex.py#L1594'>airflow/providers/google/cloud/operators/dataplex.py:1594:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataplex.py#L1733'>airflow/providers/google/cloud/operators/dataplex.py:1733:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataproc.py#L1498'>airflow/providers/google/cloud/operators/dataproc.py:1498:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/operators/dataproc.py#L2647'>airflow/providers/google/cloud/operators/dataproc.py:2647:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/615cddf427081bdbafc9437569946b16390deddb/airflow/providers/google/cloud/sensors/gcs.py#L551'>airflow/providers/google/cloud/sensors/gcs.py:551:13:</a> DOC201 `return` is not documented in docstring
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/fcf04502949b58fbcd7225ec7d10e9c73ae316d5/superset/db_engine_specs/base.py#L1992'>superset/db_engine_specs/base.py:1992:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/fcf04502949b58fbcd7225ec7d10e9c73ae316d5/superset/db_engine_specs/base.py#L2103'>superset/db_engine_specs/base.py:2103:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/fcf04502949b58fbcd7225ec7d10e9c73ae316d5/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L105'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:105:5:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L234'>src/bokeh/application/application.py:234:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L247'>src/bokeh/application/application.py:247:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L254'>src/bokeh/application/handlers/directory.py:254:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L270'>src/bokeh/application/handlers/directory.py:270:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L105'>src/bokeh/application/handlers/lifecycle.py:105:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L115'>src/bokeh/application/handlers/lifecycle.py:115:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L125'>src/bokeh/application/handlers/lifecycle.py:125:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L90'>src/bokeh/application/handlers/lifecycle.py:90:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/websocket.py#L71'>src/bokeh/client/websocket.py:71:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L262'>src/bokeh/server/views/ws.py:262:9:</a> DOC201 `return` is not documented in docstring
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/7424d4e72165b29e25c1873c06a9b45509f68039/zerver/lib/cache.py#L261'>zerver/lib/cache.py:261:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/7424d4e72165b29e25c1873c06a9b45509f68039/zerver/tests/test_openapi.py#L466'>zerver/tests/test_openapi.py:466:13:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 67 | 0 | 67 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-08-27 15:41_

There's some situations in the ecosystem check here where the diagnostic is going away and it _looks_ like a false negative is introduced, such as <https://github.com/apache/airflow/blob/6c878866dedc182c55e18e490bff3f3a9e9c5747/airflow/providers/airbyte/operators/airbyte.py#L126>. The reason there for the error going away is that the function is typed as always returning `None` -- looking at the function, I think it's pretty clear that it _doesn't_ always return `None`, but I guess that's a bug in their type annotations rather than a bug in the logic of this PR. I think it's correct for us to believe people's type annotations in this rule.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-27 15:46_

---

_@AlexWaygood approved on 2024-08-27 15:56_

Thanks!

---

_Label `docstring` added by @AlexWaygood on 2024-08-27 15:57_

---

_Label `preview` added by @AlexWaygood on 2024-08-27 15:57_

---

_Label `bug` added by @AlexWaygood on 2024-08-27 15:57_

---

_Merged by @AlexWaygood on 2024-08-27 16:00_

---

_Closed by @AlexWaygood on 2024-08-27 16:00_

---

_Comment by @codspeed-hq[bot] on 2024-08-27 16:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:doc201-explicit-none)

### Merging #13064 will **degrade performances by 9.33%**

<sub>Comparing <code>tjkuson:doc201-explicit-none</code> (bb48a09) with <code>main</code> (e6d0c4a)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/tjkuson:doc201-explicit-none)._

### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:doc201-explicit-none` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 821 µs | 905.5 µs | -9.33% |


---

_Comment by @AlexWaygood on 2024-08-27 16:03_

> Merging #13064 will **degrade performances by 9.33%**

Looks spurious; it's been very unreliable recently sadly

---

_Branch deleted on 2024-08-27 16:33_

---

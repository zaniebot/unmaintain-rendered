```yaml
number: 11074
title: "[flake8-return] Only add return None at end of function (RET503)"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: ret503-only-add-return-at-end-of-function
created_at: 2024-04-21T16:27:48Z
updated_at: 2024-08-14T10:04:16Z
url: https://github.com/astral-sh/ruff/pull/11074
synced_at: 2026-01-12T15:55:34Z
```

# [flake8-return] Only add return None at end of function (RET503)

---

_@JonathanPlasse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Fix #5765

In `RET503` documentation, it is said that it only adds `return None` at the end of functions.
The current behavior can add `return None` in multiple place in the function, but not at the end of the function.
This behavior does not match the documentation and goes against other `flake8-return` rules like `RET505` and caused people to open #5765.
The new behavior only adds `return None` at the end of functions like in the documentation and results in more readable code.
The new behavior is only available in preview mode.
The range of `RET503` is changed to be the entire function, as it will be more clear it concerns the entire function.

Before
```python
def main(value: str) -> int | None:
    if value == "one":
        return 1

    if value.startswith("do_stuff"):
        print("doing stuff")

        if value.endswith("haha"):
            print("haha")
            return None
        else:
            print("Instruction unclear.")
            return None
    else:
        print("Doing nothing.")
        return None
```
After
```python
def main(value: str) -> int | None:
    if value == "one":
        return 1

    if value.startswith("do_stuff"):
        print("doing stuff")

        if value.endswith("haha"):
            print("haha")
        else:
            print("Instruction unclear.")
    else:
        print("Doing nothing.")
    return None
```

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
I added a preview snapshot for `RET503`.
I compared the diff between the `RET503` stable and preview snapshot.
I checked the ecosystem reports and fixed one regression.

---

_Comment by @github-actions[bot] on 2024-04-21 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+124 -134 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+70 -78 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/io/path.py#L370'>airflow/io/path.py:370:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/io/path.py#L390'>airflow/io/path.py:390:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/jobs/triggerer_job_runner.py#L128'>airflow/jobs/triggerer_job_runner.py:128:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/jobs/triggerer_job_runner.py#L129'>airflow/jobs/triggerer_job_runner.py:129:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/metrics/otel_logger.py#L181'>airflow/metrics/otel_logger.py:181:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/metrics/otel_logger.py#L202'>airflow/metrics/otel_logger.py:202:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/metrics/otel_logger.py#L207'>airflow/metrics/otel_logger.py:207:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/metrics/otel_logger.py#L228'>airflow/metrics/otel_logger.py:228:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/models/dag.py#L3958'>airflow/models/dag.py:3958:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/models/dag.py#L3959'>airflow/models/dag.py:3959:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/models/param.py#L268'>airflow/models/param.py:268:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/models/param.py#L271'>airflow/models/param.py:271:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L273'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:273:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L284'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:284:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L285'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:285:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/operators/emr.py#L1412'>airflow/providers/amazon/aws/operators/emr.py:1412:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/operators/emr.py#L1415'>airflow/providers/amazon/aws/operators/emr.py:1415:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/operators/eventbridge.py#L61'>airflow/providers/amazon/aws/operators/eventbridge.py:61:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/amazon/aws/operators/eventbridge.py#L82'>airflow/providers/amazon/aws/operators/eventbridge.py:82:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L347'>airflow/providers/apache/beam/operators/beam.py:347:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L365'>airflow/providers/apache/beam/operators/beam.py:365:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L369'>airflow/providers/apache/beam/operators/beam.py:369:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L403'>airflow/providers/apache/beam/operators/beam.py:403:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L540'>airflow/providers/apache/beam/operators/beam.py:540:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L553'>airflow/providers/apache/beam/operators/beam.py:553:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L557'>airflow/providers/apache/beam/operators/beam.py:557:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L605'>airflow/providers/apache/beam/operators/beam.py:605:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/fcc9f3dc0eb0b9c53f59bbe6619a175779db2cbc/airflow/providers/apache/beam/operators/beam.py#L734'>airflow/providers/apache/beam/operators/beam.py:734:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
... 120 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L112'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:112:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L129'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:129:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/utils/hashing_tests.py#L59'>tests/integration_tests/utils/hashing_tests.py:59:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/utils/hashing_tests.py#L60'>tests/integration_tests/utils/hashing_tests.py:60:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/unit_tests/queries/query_object_test.py#L103'>tests/unit_tests/queries/query_object_test.py:103:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/unit_tests/queries/query_object_test.py#L104'>tests/unit_tests/queries/query_object_test.py:104:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+43 -48 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L82'>examples/server/app/clustering/main.py:82:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L83'>examples/server/app/clustering/main.py:83:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L314'>src/bokeh/core/has_props.py:314:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L340'>src/bokeh/core/has_props.py:340:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L342'>src/bokeh/core/has_props.py:342:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L367'>src/bokeh/core/has_props.py:367:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L386'>src/bokeh/core/serialization.py:386:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L409'>src/bokeh/core/serialization.py:409:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L438'>src/bokeh/core/serialization.py:438:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L469'>src/bokeh/core/serialization.py:469:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L530'>src/bokeh/core/serialization.py:530:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L562'>src/bokeh/core/serialization.py:562:21:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L572'>src/bokeh/core/serialization.py:572:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L578'>src/bokeh/core/serialization.py:578:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L710'>src/bokeh/core/serialization.py:710:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L717'>src/bokeh/core/serialization.py:717:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L723'>src/bokeh/core/serialization.py:723:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/duckdb/__init__.py#L520'>ibis/backends/duckdb/__init__.py:520:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/duckdb/__init__.py#L578'>ibis/backends/duckdb/__init__.py:578:39:</a> RUF100 Unused `noqa` directive (unused: `RET503`)
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/pyspark/__init__.py#L864'>ibis/backends/pyspark/__init__.py:864:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/pyspark/__init__.py#L910'>ibis/backends/pyspark/__init__.py:910:39:</a> RUF100 Unused `noqa` directive (unused: `RET503`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/_version_helper.py#L41'>_version_helper.py:41:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/_version_helper.py#L42'>_version_helper.py:42:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/lib/addressee.py#L137'>zerver/lib/addressee.py:137:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/lib/addressee.py#L96'>zerver/lib/addressee.py:96:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/tests/test_import_export.py#L1606'>zerver/tests/test_import_export.py:1606:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/tests/test_import_export.py#L1609'>zerver/tests/test_import_export.py:1609:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/tests/test_openapi.py#L456'>zerver/tests/test_openapi.py:456:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/tests/test_openapi.py#L535'>zerver/tests/test_openapi.py:535:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/07c927ae884445935c19c427fa4f882faddc96d5/zerver/tests/test_openapi.py#L536'>zerver/tests/test_openapi.py:536:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET503 | 256 | 122 | 134 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-22 14:21_

---

_@JonathanPlasse reviewed on 2024-04-24 16:32_

---

_Review comment by @JonathanPlasse on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:482 on 2024-04-24 16:32_

Early return if in preview mode because we only need to know if the function has at least one implicit return to add a single `return None` at the end of the function.
The stable version can add multiple return so it cannot return early.
The code should be simplified once the preview version become stable.

---

_Comment by @JonathanPlasse on 2024-04-24 16:33_

A comment should be added that when the preview version becomes stable the code can be simplified with early return everywhere.

---

_Comment by @JonathanPlasse on 2024-06-27 13:03_

Will it be included in Ruff 0.5.0?

---

_Comment by @AlexWaygood on 2024-06-27 15:05_

> Will it be included in Ruff 0.5.0?

Just seen this -- unfortunately I just set the release workflow in motion, so unfortunately not. Sorry :-(

---

_Comment by @JonathanPlasse on 2024-06-27 15:16_

No worries.

---

_Comment by @JonathanPlasse on 2024-08-13 12:15_

Will it be included in Ruff 0.6.0?

---

_Label `preview` added by @MichaReiser on 2024-08-13 15:24_

---

_Label `fixes` added by @MichaReiser on 2024-08-13 15:24_

---

_Comment by @codspeed-hq[bot] on 2024-08-13 15:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/JonathanPlasse:ret503-only-add-return-at-end-of-function)

### Merging #11074 will **not alter performance**

<sub>Comparing <code>JonathanPlasse:ret503-only-add-return-at-end-of-function</code> (4e575bc) with <code>main</code> (2520ebb)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @MichaReiser on 2024-08-13 15:44_

I don't think this requires a minor release, considering that the change is gated behind preview mode.

---

_@MichaReiser reviewed on 2024-08-13 15:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:563 on 2024-08-13 15:47_

I don't have a great solution but I do find it a bit surprising that `has_implicit_return` adds diagnostics in the non preview mode. 

I'm somewhat inclined to duplicate the logic between preview and non-preview mode. That should also make it much easier when stabilizing the new behavior (and it's not that much code that needs copying). Unless you see a way of how we can avoid the side-effects of `has_implicit_return` in stable (e.g. could it return a `Vec` with all the statements for which diagnostics should be added?)

---

_@MichaReiser reviewed on 2024-08-13 19:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:563 on 2024-08-13 19:54_

I don't mind going ahead with the PR as is but I'm interested to hear what you think of duplicating the logic OR maybe you have another idea to make this a bit clearer 

---

_@JonathanPlasse reviewed on 2024-08-14 06:01_

---

_Review comment by @JonathanPlasse on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:563 on 2024-08-14 06:01_

I used the `Vec` solution to remove the side effects. 
The code is nicer.
Thank you for suggesting it.

---

_@MichaReiser reviewed on 2024-08-14 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:563 on 2024-08-14 07:42_

Thanks. Will merge now and sorry for the long wait

---

_Merged by @MichaReiser on 2024-08-14 07:47_

---

_Closed by @MichaReiser on 2024-08-14 07:47_

---

_Branch deleted on 2024-08-14 09:57_

---

_@JonathanPlasse reviewed on 2024-08-14 10:04_

---

_Review comment by @JonathanPlasse on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:563 on 2024-08-14 10:04_

You are welcome

---

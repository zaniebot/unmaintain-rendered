```yaml
number: 10276
title: "Formatter: Improve single-with item formatting for Python 3.8 or older"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: single-with-item-pre-39
created_at: 2024-03-07T15:30:16Z
updated_at: 2024-03-08T23:59:51Z
url: https://github.com/astral-sh/ruff/pull/10276
synced_at: 2026-01-12T15:55:31Z
```

# Formatter: Improve single-with item formatting for Python 3.8 or older

---

_@MichaReiser_

## Summary

This PR changes how we format `with` statements with a single with item for Python 3.8 or older. This change is not compatible with Black.

This is how we format a single-item with statement today 

```python
def run(data_path, model_uri):
    with pyspark.sql.SparkSession.builder.config(
        key="spark.python.worker.reuse", value=True
    ).config(key="spark.ui.enabled", value=False).master(
        "local-cluster[2, 1, 1024]"
    ).getOrCreate():
        # ignore spark log output
        spark.sparkContext.setLogLevel("OFF")
        print(score_model(spark, data_path, model_uri))
```

This is different than how we would format the same expression if it is inside any other clause header (`while`, `if`, ...):

```python
def run(data_path, model_uri):
    while (
        pyspark.sql.SparkSession.builder.config(
            key="spark.python.worker.reuse", value=True
        )
        .config(key="spark.ui.enabled", value=False)
        .master("local-cluster[2, 1, 1024]")
        .getOrCreate()
    ):
        # ignore spark log output
        spark.sparkContext.setLogLevel("OFF")
        print(score_model(spark, data_path, model_uri))

```

Which seems inconsistent to me. 

This PR changes the formatting of the single-item with Python 3.8 or older to match that of other clause headers.

```python
def run(data_path, model_uri):
    with (
        pyspark.sql.SparkSession.builder.config(
            key="spark.python.worker.reuse", value=True
        )
        .config(key="spark.ui.enabled", value=False)
        .master("local-cluster[2, 1, 1024]")
        .getOrCreate()
    ):
        # ignore spark log output
        spark.sparkContext.setLogLevel("OFF")
        print(score_model(spark, data_path, model_uri))
```

According to our versioning policy, this style change is gated behind a preview flag.

## Test Plan

See added tests.

Added 

---

_Label `formatter` added by @MichaReiser on 2024-03-07 15:30_

---

_Label `preview` added by @MichaReiser on 2024-03-07 15:30_

---

_Comment by @github-actions[bot] on 2024-03-07 15:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+2 -2 lines in 1 file in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/0e9b5bc8521c98ece568854ed7f45aa8c6c9411f/rotkehlchen/rotkehlchen.py#L1213'>rotkehlchen/rotkehlchen.py~L1213</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with (
-            contextlib.suppress(UnknownAsset)
+        with contextlib.suppress(
+            UnknownAsset
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+8 -7 lines in 2 files in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+6 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/13f5cf13e1a2bfd8c9d699534dc7211c5fd1ca93/examples/flower_classifier/score_images_spark.py#L81'>examples/flower_classifier/score_images_spark.py~L81</a>
```diff
 @cli_args.MODEL_URI
 @click.argument("data-path")
 def run(data_path, model_uri):
-    with pyspark.sql.SparkSession.builder.config(
-        key="spark.python.worker.reuse", value=True
-    ).config(key="spark.ui.enabled", value=False).master(
-        "local-cluster[2, 1, 1024]"
-    ).getOrCreate() as spark:
+    with (
+        pyspark.sql.SparkSession.builder.config(key="spark.python.worker.reuse", value=True)
+        .config(key="spark.ui.enabled", value=False)
+        .master("local-cluster[2, 1, 1024]")
+        .getOrCreate()
+    ) as spark:
         # ignore spark log output
         spark.sparkContext.setLogLevel("OFF")
         print(score_model(spark, data_path, model_uri))
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/0e9b5bc8521c98ece568854ed7f45aa8c6c9411f/rotkehlchen/rotkehlchen.py#L1211'>rotkehlchen/rotkehlchen.py~L1211</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with (
-            contextlib.suppress(UnknownAsset)
+        with contextlib.suppress(
+            UnknownAsset
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```

</p>
</details>




---

_@MichaReiser reviewed on 2024-03-07 17:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:727 on 2024-03-07 17:45_

This change might be controversial. I would say it's not ideal but the best we can do with the limitations given by Python 3.8, other than not parenthesizing and exceeding the configured line width. 

---

_Renamed from "single with item pre 39" to "Formatter: Improve single-with item formatting for Python 3.8 or older" by @MichaReiser on 2024-03-07 17:46_

---

_Marked ready for review by @MichaReiser on 2024-03-08 11:40_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-08 16:12_

---

_@charliermarsh approved on 2024-03-08 23:50_

This seems reasonable to me.

---

_Merged by @charliermarsh on 2024-03-08 23:56_

---

_Closed by @charliermarsh on 2024-03-08 23:56_

---

_Branch deleted on 2024-03-08 23:56_

---

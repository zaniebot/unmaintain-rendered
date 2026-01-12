```yaml
number: 14729
title: "Improve error messages and docs for `flake8-comprehensions` rules"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: alex/comp-docs
created_at: 2024-12-02T13:26:25Z
updated_at: 2024-12-02T13:36:18Z
url: https://github.com/astral-sh/ruff/pull/14729
synced_at: 2026-01-12T15:55:48Z
```

# Improve error messages and docs for `flake8-comprehensions` rules

---

_@AlexWaygood_

## Summary

This PR attempts to improve the docs and error messages for our `flake8-comprehension` rules. Along the way I saw various opportunities for cleanups and micro-optimisations, but the only _significant_ changes should be to the error messages and docs.

The main change is to remove quite a few backticks which don't seem necessary and, I think, contributed to some of the confusion in https://github.com/astral-sh/ruff/issues/14644. For example, rather than referring to "`set` comprehensions", refer to "set comprehensions". This reads more naturally, and it's harder to misunderstand it as saying "use a call to `set()`", which is how it was misread by the OP in https://github.com/astral-sh/ruff/issues/14644.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `documentation` added by @AlexWaygood on 2024-12-02 13:26_

---

_Comment by @github-actions[bot] on 2024-12-02 13:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3239 -3239 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+928 -928 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_connexion/endpoints/dag_stats_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_stats_endpoint.py:58:21:</a> C414 Unnecessary `list()` call within `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_connexion/endpoints/dag_stats_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_stats_endpoint.py:58:21:</a> C414 Unnecessary `list` call within `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/public/dags.py#L139'>airflow/api_fastapi/core_api/routes/public/dags.py:139:42:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/public/dags.py#L139'>airflow/api_fastapi/core_api/routes/public/dags.py:139:42:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/ui/config.py#L69'>airflow/api_fastapi/core_api/routes/ui/config.py:69:19:</a> C416 Unnecessary `dict` comprehension (rewrite using `dict()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/ui/config.py#L69'>airflow/api_fastapi/core_api/routes/ui/config.py:69:19:</a> C416 Unnecessary dict comprehension (rewrite using `dict()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/backfill_command.py#L65'>airflow/cli/commands/backfill_command.py:65:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/backfill_command.py#L65'>airflow/cli/commands/backfill_command.py:65:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/variable_command.py#L113'>airflow/cli/commands/variable_command.py:113:47:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/variable_command.py#L113'>airflow/cli/commands/variable_command.py:113:47:</a> C413 [*] Unnecessary `list` call around `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/collection.py#L481'>airflow/dag_processing/collection.py:481:34:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/collection.py#L481'>airflow/dag_processing/collection.py:481:34:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
... 31 additional changes omitted for rule C416
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/manager.py#L1370'>airflow/dag_processing/manager.py:1370:26:</a> C400 Unnecessary generator (rewrite as a `list` comprehension)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/manager.py#L1370'>airflow/dag_processing/manager.py:1370:26:</a> C400 Unnecessary generator (rewrite as a list comprehension)
... 1842 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+95 -95 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_address.py#L68'>superset/advanced_data_type/plugins/internet_address.py:68:17:</a> C417 Unnecessary `map()` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_address.py#L68'>superset/advanced_data_type/plugins/internet_address.py:68:17:</a> C417 Unnecessary `map` usage (rewrite using a generator expression)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_port.py#L97'>superset/advanced_data_type/plugins/internet_port.py:97:17:</a> C417 Unnecessary `map()` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_port.py#L97'>superset/advanced_data_type/plugins/internet_port.py:97:17:</a> C417 Unnecessary `map` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L175'>superset/charts/post_processing.py:175:33:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L175'>superset/charts/post_processing.py:175:33:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L192'>superset/charts/post_processing.py:192:33:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L192'>superset/charts/post_processing.py:192:33:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/commands/exceptions.py#L74'>superset/commands/exceptions.py:74:16:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/commands/exceptions.py#L74'>superset/commands/exceptions.py:74:16:</a> C413 [*] Unnecessary `list` call around `sorted()`
... 180 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+948 -948 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py#L6'>docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py:6:32:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py#L6'>docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py:6:32:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L29'>examples/advanced/extensions/parallel_plot/parallel_plot.py:29:36:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L29'>examples/advanced/extensions/parallel_plot/parallel_plot.py:29:36:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L72'>examples/advanced/extensions/parallel_plot/parallel_plot.py:72:31:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L72'>examples/advanced/extensions/parallel_plot/parallel_plot.py:72:31:</a> C408 Unnecessary `dict` call (rewrite as a literal)
... 1857 additional changes omitted for rule C408
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L17'>examples/basic/annotations/whisker.py:17:11:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L17'>examples/basic/annotations/whisker.py:17:11:</a> C413 [*] Unnecessary `list` call around `sorted()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L24'>examples/output/webgl/clustering.py:24:19:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L24'>examples/output/webgl/clustering.py:24:19:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/filtering.py#L47'>examples/plotting/filtering.py:47:30:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/filtering.py#L47'>examples/plotting/filtering.py:47:30:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/burtin.py#L25'>examples/topics/pie/burtin.py:25:8:</a> C406 Unnecessary `list` literal (rewrite as a `dict` literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/burtin.py#L25'>examples/topics/pie/burtin.py:25:8:</a> C406 Unnecessary list literal (rewrite as a dict literal)
... 1882 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L274'>src/latch/account.py:274:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L274'>src/latch/account.py:274:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L312'>src/latch/account.py:312:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L312'>src/latch/account.py:312:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/ldata/_transfer/progress.py#L48'>src/latch/ldata/_transfer/progress.py:48:29:</a> C416 Unnecessary `set` comprehension (rewrite using `set()`)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/ldata/_transfer/progress.py#L48'>src/latch/ldata/_transfer/progress.py:48:29:</a> C416 Unnecessary set comprehension (rewrite using `set()`)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/project.py#L245'>src/latch/registry/project.py:245:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/project.py#L245'>src/latch/registry/project.py:245:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
... 9 additional changes omitted for rule C409
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/table.py#L637'>src/latch/registry/table.py:637:27:</a> C400 Unnecessary generator (rewrite as a `list` comprehension)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/table.py#L637'>src/latch/registry/table.py:637:27:</a> C400 Unnecessary generator (rewrite as a list comprehension)
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L101'>examples/bulk_import/example_bulkwriter.py:101:33:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L101'>examples/bulk_import/example_bulkwriter.py:101:33:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L314'>examples/bulk_import/example_bulkwriter.py:314:30:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L314'>examples/bulk_import/example_bulkwriter.py:314:30:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L341'>examples/bulk_import/example_bulkwriter.py:341:39:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L341'>examples/bulk_import/example_bulkwriter.py:341:39:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L42'>examples/concurrency/multithreading_hello_milvus.py:42:23:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L42'>examples/concurrency/multithreading_hello_milvus.py:42:23:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L53'>examples/concurrency/multithreading_hello_milvus.py:53:13:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L53'>examples/concurrency/multithreading_hello_milvus.py:53:13:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
... 38 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1233 -1233 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/migrations/0015_clear_duplicate_counts.py#L23'>analytics/migrations/0015_clear_duplicate_counts.py:23:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/migrations/0015_clear_duplicate_counts.py#L23'>analytics/migrations/0015_clear_duplicate_counts.py:23:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L83'>analytics/views/stats.py:83:19:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L83'>analytics/views/stats.py:83:19:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L94'>analytics/views/stats.py:94:17:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L94'>analytics/views/stats.py:94:17:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L122'>corporate/lib/activity.py:122:45:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L122'>corporate/lib/activity.py:122:45:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L129'>corporate/lib/activity.py:129:46:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L129'>corporate/lib/activity.py:129:46:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L136'>corporate/lib/activity.py:136:43:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L136'>corporate/lib/activity.py:136:43:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L163'>corporate/lib/activity.py:163:57:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L163'>corporate/lib/activity.py:163:57:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L59'>corporate/lib/activity.py:59:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L59'>corporate/lib/activity.py:59:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L63'>corporate/lib/activity.py:63:12:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L63'>corporate/lib/activity.py:63:12:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L69'>corporate/lib/activity.py:69:9:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
... 2447 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (13 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C408 | 6062 | 3031 | 3031 | 0 | 0 |
| C416 | 126 | 63 | 63 | 0 | 0 |
| C403 | 60 | 30 | 30 | 0 | 0 |
| C401 | 58 | 29 | 29 | 0 | 0 |
| C417 | 40 | 20 | 20 | 0 | 0 |
| C400 | 36 | 18 | 18 | 0 | 0 |
| C414 | 30 | 15 | 15 | 0 | 0 |
| C409 | 30 | 15 | 15 | 0 | 0 |
| C413 | 12 | 6 | 6 | 0 | 0 |
| C418 | 12 | 6 | 6 | 0 | 0 |
| C405 | 4 | 2 | 2 | 0 | 0 |
| C402 | 4 | 2 | 2 | 0 | 0 |
| C406 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3239 -3239 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+928 -928 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_connexion/endpoints/dag_stats_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_stats_endpoint.py:58:21:</a> C414 Unnecessary `list()` call within `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_connexion/endpoints/dag_stats_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_stats_endpoint.py:58:21:</a> C414 Unnecessary `list` call within `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/public/dags.py#L139'>airflow/api_fastapi/core_api/routes/public/dags.py:139:42:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/public/dags.py#L139'>airflow/api_fastapi/core_api/routes/public/dags.py:139:42:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/ui/config.py#L69'>airflow/api_fastapi/core_api/routes/ui/config.py:69:19:</a> C416 Unnecessary `dict` comprehension (rewrite using `dict()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/api_fastapi/core_api/routes/ui/config.py#L69'>airflow/api_fastapi/core_api/routes/ui/config.py:69:19:</a> C416 Unnecessary dict comprehension (rewrite using `dict()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/backfill_command.py#L65'>airflow/cli/commands/backfill_command.py:65:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/backfill_command.py#L65'>airflow/cli/commands/backfill_command.py:65:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/variable_command.py#L113'>airflow/cli/commands/variable_command.py:113:47:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/cli/commands/variable_command.py#L113'>airflow/cli/commands/variable_command.py:113:47:</a> C413 [*] Unnecessary `list` call around `sorted()`
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/collection.py#L481'>airflow/dag_processing/collection.py:481:34:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/collection.py#L481'>airflow/dag_processing/collection.py:481:34:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
... 31 additional changes omitted for rule C416
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/manager.py#L1370'>airflow/dag_processing/manager.py:1370:26:</a> C400 Unnecessary generator (rewrite as a `list` comprehension)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/dag_processing/manager.py#L1370'>airflow/dag_processing/manager.py:1370:26:</a> C400 Unnecessary generator (rewrite as a list comprehension)
... 1842 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+95 -95 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_address.py#L68'>superset/advanced_data_type/plugins/internet_address.py:68:17:</a> C417 Unnecessary `map()` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_address.py#L68'>superset/advanced_data_type/plugins/internet_address.py:68:17:</a> C417 Unnecessary `map` usage (rewrite using a generator expression)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_port.py#L97'>superset/advanced_data_type/plugins/internet_port.py:97:17:</a> C417 Unnecessary `map()` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/advanced_data_type/plugins/internet_port.py#L97'>superset/advanced_data_type/plugins/internet_port.py:97:17:</a> C417 Unnecessary `map` usage (rewrite using a generator expression)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L175'>superset/charts/post_processing.py:175:33:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L175'>superset/charts/post_processing.py:175:33:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L192'>superset/charts/post_processing.py:192:33:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/charts/post_processing.py#L192'>superset/charts/post_processing.py:192:33:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
+ <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/commands/exceptions.py#L74'>superset/commands/exceptions.py:74:16:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/apache/superset/blob/3d3c09d299ccb43a6582f0bedadc844a07e65311/superset/commands/exceptions.py#L74'>superset/commands/exceptions.py:74:16:</a> C413 [*] Unnecessary `list` call around `sorted()`
... 180 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+948 -948 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py#L6'>docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py:6:32:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py#L6'>docs/bokeh/source/docs/first_steps/examples/first_steps_8_filter.py:6:32:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L29'>examples/advanced/extensions/parallel_plot/parallel_plot.py:29:36:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L29'>examples/advanced/extensions/parallel_plot/parallel_plot.py:29:36:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L72'>examples/advanced/extensions/parallel_plot/parallel_plot.py:72:31:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L72'>examples/advanced/extensions/parallel_plot/parallel_plot.py:72:31:</a> C408 Unnecessary `dict` call (rewrite as a literal)
... 1857 additional changes omitted for rule C408
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L17'>examples/basic/annotations/whisker.py:17:11:</a> C413 [*] Unnecessary `list()` call around `sorted()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L17'>examples/basic/annotations/whisker.py:17:11:</a> C413 [*] Unnecessary `list` call around `sorted()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L24'>examples/output/webgl/clustering.py:24:19:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L24'>examples/output/webgl/clustering.py:24:19:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/filtering.py#L47'>examples/plotting/filtering.py:47:30:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/filtering.py#L47'>examples/plotting/filtering.py:47:30:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/burtin.py#L25'>examples/topics/pie/burtin.py:25:8:</a> C406 Unnecessary `list` literal (rewrite as a `dict` literal)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/burtin.py#L25'>examples/topics/pie/burtin.py:25:8:</a> C406 Unnecessary list literal (rewrite as a dict literal)
... 1882 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L274'>src/latch/account.py:274:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L274'>src/latch/account.py:274:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L312'>src/latch/account.py:312:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/account.py#L312'>src/latch/account.py:312:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/ldata/_transfer/progress.py#L48'>src/latch/ldata/_transfer/progress.py:48:29:</a> C416 Unnecessary `set` comprehension (rewrite using `set()`)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/ldata/_transfer/progress.py#L48'>src/latch/ldata/_transfer/progress.py:48:29:</a> C416 Unnecessary set comprehension (rewrite using `set()`)
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/project.py#L245'>src/latch/registry/project.py:245:25:</a> C409 Unnecessary `list` literal passed to `tuple()` (rewrite as a `tuple` literal)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/project.py#L245'>src/latch/registry/project.py:245:25:</a> C409 Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
... 9 additional changes omitted for rule C409
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/table.py#L637'>src/latch/registry/table.py:637:27:</a> C400 Unnecessary generator (rewrite as a `list` comprehension)
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/table.py#L637'>src/latch/registry/table.py:637:27:</a> C400 Unnecessary generator (rewrite as a list comprehension)
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L101'>examples/bulk_import/example_bulkwriter.py:101:33:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L101'>examples/bulk_import/example_bulkwriter.py:101:33:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L314'>examples/bulk_import/example_bulkwriter.py:314:30:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L314'>examples/bulk_import/example_bulkwriter.py:314:30:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L341'>examples/bulk_import/example_bulkwriter.py:341:39:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/bulk_import/example_bulkwriter.py#L341'>examples/bulk_import/example_bulkwriter.py:341:39:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L42'>examples/concurrency/multithreading_hello_milvus.py:42:23:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L42'>examples/concurrency/multithreading_hello_milvus.py:42:23:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
- <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L53'>examples/concurrency/multithreading_hello_milvus.py:53:13:</a> C416 Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/milvus-io/pymilvus/blob/bd0cbff639e9b87618336b4c4125a4bfecc8eb71/examples/concurrency/multithreading_hello_milvus.py#L53'>examples/concurrency/multithreading_hello_milvus.py:53:13:</a> C416 Unnecessary list comprehension (rewrite using `list()`)
... 38 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1233 -1233 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/migrations/0015_clear_duplicate_counts.py#L23'>analytics/migrations/0015_clear_duplicate_counts.py:23:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/migrations/0015_clear_duplicate_counts.py#L23'>analytics/migrations/0015_clear_duplicate_counts.py:23:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L83'>analytics/views/stats.py:83:19:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L83'>analytics/views/stats.py:83:19:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L94'>analytics/views/stats.py:94:17:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/analytics/views/stats.py#L94'>analytics/views/stats.py:94:17:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L122'>corporate/lib/activity.py:122:45:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L122'>corporate/lib/activity.py:122:45:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L129'>corporate/lib/activity.py:129:46:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L129'>corporate/lib/activity.py:129:46:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L136'>corporate/lib/activity.py:136:43:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L136'>corporate/lib/activity.py:136:43:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L163'>corporate/lib/activity.py:163:57:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L163'>corporate/lib/activity.py:163:57:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L59'>corporate/lib/activity.py:59:20:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L59'>corporate/lib/activity.py:59:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L63'>corporate/lib/activity.py:63:12:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
- <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L63'>corporate/lib/activity.py:63:12:</a> C408 Unnecessary `dict` call (rewrite as a literal)
+ <a href='https://github.com/zulip/zulip/blob/a35addda73647558c245e37132aec4dc29f230a3/corporate/lib/activity.py#L69'>corporate/lib/activity.py:69:9:</a> C408 Unnecessary `dict()` call (rewrite as a literal)
... 2447 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (13 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C408 | 6062 | 3031 | 3031 | 0 | 0 |
| C416 | 126 | 63 | 63 | 0 | 0 |
| C403 | 60 | 30 | 30 | 0 | 0 |
| C401 | 58 | 29 | 29 | 0 | 0 |
| C417 | 40 | 20 | 20 | 0 | 0 |
| C400 | 36 | 18 | 18 | 0 | 0 |
| C414 | 30 | 15 | 15 | 0 | 0 |
| C409 | 30 | 15 | 15 | 0 | 0 |
| C413 | 12 | 6 | 6 | 0 | 0 |
| C418 | 12 | 6 | 6 | 0 | 0 |
| C405 | 4 | 2 | 2 | 0 | 0 |
| C402 | 4 | 2 | 2 | 0 | 0 |
| C406 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-12-02 13:36_

No changes in the errors being emitted in the ecosystem check, only in the error messages

---

_Merged by @AlexWaygood on 2024-12-02 13:36_

---

_Closed by @AlexWaygood on 2024-12-02 13:36_

---

_Branch deleted on 2024-12-02 13:36_

---

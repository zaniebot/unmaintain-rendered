```yaml
number: 5275
title: "Avoid `mutable-class-default` (`RUF012`) for fully untyped classes"
type: pull_request
state: closed
author: charliermarsh
labels:
  - needs-decision
assignees: []
base: main
head: charlie/typing-only
created_at: 2023-06-22T00:24:38Z
updated_at: 2024-01-26T19:34:47Z
url: https://github.com/astral-sh/ruff/pull/5275
synced_at: 2026-01-10T22:57:09Z
```

# Avoid `mutable-class-default` (`RUF012`) for fully untyped classes

---

_Pull request opened by @charliermarsh on 2023-06-22 00:24_

## Summary

This PR attempts to avoid flagging mutable class attributes (and suggesting that they be annotated with `ClassVar`) for classes that are fully untyped. I'm somewhat undecided on it (the underlying _issue_ is still present, but the suggested fix isn't applicable), but you can see the discussion in #5243.


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/mutable_class_default.rs`:55 on 2023-06-22 06:45_

This changes the runtime of this rule to always iterate over the body twice. I don't know if this is a big issue, considering that classes only have a few top-level statements (it skips over all function bodies). 

However, I don't have a good idea that is guaranteed to be faster:

* Collecting the diagnostics and ignoring them if no assignment statement was seen is probably slower it requires allocating the diagnostics `Vec` and the messages in `DiagnosticKind`. 
* Filtering out the `AnnAssignStmt` and `AnnAssign` statements and storing them in a `Vec`. Running `is_any` and the check only over that sublist. Again, the allocation may be as expensive as iterating over all statements. 

---

_@MichaReiser approved on 2023-06-22 06:45_

---

_Comment by @aberres on 2023-06-30 11:47_

I think this would solve most of the false positives in our (mostly) typed codebase.

---

_Label `needs-decision` added by @zanieb on 2023-10-19 16:20_

---

_Comment by @zanieb on 2023-10-19 16:25_

There's no other way to indicate that a mutable class default is the desired behavior, is there? Could we look for mutation of the variable outside of `@classmethod` annotated functions? 😬 

---

_Comment by @github-actions[bot] on 2023-12-12 22:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -1661 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/twilio_voice.py#L21'>rasa/core/channels/twilio_voice.py:21:27:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/twilio_voice.py#L91'>rasa/core/channels/twilio_voice.py:91:34:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/evaluation/marker_tracker_loader.py#L31'>rasa/core/evaluation/marker_tracker_loader.py:31:24:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/precomputation.py#L60'>rasa/core/featurizers/precomputation.py:60:64:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -402 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/api_connexion/schemas/common_schema.py#L106'>airflow/api_connexion/schemas/common_schema.py:106:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/example_dags/plugins/listener_plugin.py#L26'>airflow/example_dags/plugins/listener_plugin.py:26:17:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/example_dags/plugins/workday.py#L103'>airflow/example_dags/plugins/workday.py:103:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L309'>airflow/operators/python.py:309:38:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L330'>airflow/operators/python.py:330:42:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L343'>airflow/operators/python.py:343:41:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L71'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:71:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L256'>airflow/providers/amazon/aws/hooks/emr.py:256:31:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L257'>airflow/providers/amazon/aws/hooks/emr.py:257:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L258'>airflow/providers/amazon/aws/hooks/emr.py:258:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L261'>airflow/providers/amazon/aws/hooks/emr.py:261:39:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L262'>airflow/providers/amazon/aws/hooks/emr.py:262:34:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 390 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/font-awesome/fontawesome_icon.py#L11'>examples/advanced/extensions/font-awesome/fontawesome_icon.py:11:24:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/widget.py#L15'>examples/advanced/extensions/widget.py:15:22:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/visual.py#L84'>src/bokeh/core/property/visual.py:84:22:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/glyphs.py#L797'>src/bokeh/models/glyphs.py:797:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_color.py#L74'>src/bokeh/sphinxext/bokeh_color.py:74:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_enum.py#L95'>src/bokeh/sphinxext/bokeh_enum.py:95:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_example_metadata.py#L70'>src/bokeh/sphinxext/bokeh_example_metadata.py:70:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_jinja.py#L77'>src/bokeh/sphinxext/bokeh_jinja.py:77:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_model.py#L105'>src/bokeh/sphinxext/bokeh_model.py:105:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_options.py#L96'>src/bokeh/sphinxext/bokeh_options.py:96:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9ca69c5eb5c7718f9ff41be13603b17b6c384593/tools/pylint/log_checker.py#L30'>tools/pylint/log_checker.py:30:15:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/rotki/rotki/blob/9ca69c5eb5c7718f9ff41be13603b17b6c384593/tools/pylint/not_checker.py#L20'>tools/pylint/not_checker.py:20:15:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1231 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0001_initial.py#L12'>analytics/migrations/0001_initial.py:12:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0001_initial.py#L7'>analytics/migrations/0001_initial.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0002_remove_huddlecount.py#L5'>analytics/migrations/0002_remove_huddlecount.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0002_remove_huddlecount.py#L9'>analytics/migrations/0002_remove_huddlecount.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0003_fillstate.py#L5'>analytics/migrations/0003_fillstate.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0003_fillstate.py#L9'>analytics/migrations/0003_fillstate.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0004_add_subgroup.py#L5'>analytics/migrations/0004_add_subgroup.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0004_add_subgroup.py#L9'>analytics/migrations/0004_add_subgroup.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0005_alter_field_size.py#L5'>analytics/migrations/0005_alter_field_size.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0005_alter_field_size.py#L9'>analytics/migrations/0005_alter_field_size.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0006_add_subgroup_to_unique_constraints.py#L5'>analytics/migrations/0006_add_subgroup_to_unique_constraints.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0006_add_subgroup_to_unique_constraints.py#L9'>analytics/migrations/0006_add_subgroup_to_unique_constraints.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0007_remove_interval.py#L10'>analytics/migrations/0007_remove_interval.py:10:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0007_remove_interval.py#L6'>analytics/migrations/0007_remove_interval.py:6:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0008_add_count_indexes.py#L11'>analytics/migrations/0008_add_count_indexes.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0008_add_count_indexes.py#L6'>analytics/migrations/0008_add_count_indexes.py:6:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0009_remove_messages_to_stream_stat.py#L24'>analytics/migrations/0009_remove_messages_to_stream_stat.py:24:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0009_remove_messages_to_stream_stat.py#L28'>analytics/migrations/0009_remove_messages_to_stream_stat.py:28:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0010_clear_messages_sent_values.py#L24'>analytics/migrations/0010_clear_messages_sent_values.py:24:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0010_clear_messages_sent_values.py#L26'>analytics/migrations/0010_clear_messages_sent_values.py:26:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0011_clear_analytics_tables.py#L21'>analytics/migrations/0011_clear_analytics_tables.py:21:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0011_clear_analytics_tables.py#L25'>analytics/migrations/0011_clear_analytics_tables.py:25:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0012_add_on_delete.py#L12'>analytics/migrations/0012_add_on_delete.py:12:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0012_add_on_delete.py#L8'>analytics/migrations/0012_add_on_delete.py:8:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0013_remove_anomaly.py#L11'>analytics/migrations/0013_remove_anomaly.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0013_remove_anomaly.py#L7'>analytics/migrations/0013_remove_anomaly.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0014_remove_fillstate_last_modified.py#L11'>analytics/migrations/0014_remove_fillstate_last_modified.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0014_remove_fillstate_last_modified.py#L7'>analytics/migrations/0014_remove_fillstate_last_modified.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0015_clear_duplicate_counts.py#L58'>analytics/migrations/0015_clear_duplicate_counts.py:58:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0015_clear_duplicate_counts.py#L62'>analytics/migrations/0015_clear_duplicate_counts.py:62:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0016_unique_constraint_when_subgroup_null.py#L11'>analytics/migrations/0016_unique_constraint_when_subgroup_null.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0016_unique_constraint_when_subgroup_null.py#L7'>analytics/migrations/0016_unique_constraint_when_subgroup_null.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0017_regenerate_partial_indexes.py#L17'>analytics/migrations/0017_regenerate_partial_indexes.py:17:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0017_regenerate_partial_indexes.py#L5'>analytics/migrations/0017_regenerate_partial_indexes.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L106'>analytics/models.py:106:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L120'>analytics/models.py:120:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 1195 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF012 | 1661 | 0 | 1661 | 0 | 0 |
| RUF100 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -1563 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/twilio_voice.py#L21'>rasa/core/channels/twilio_voice.py:21:27:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/twilio_voice.py#L91'>rasa/core/channels/twilio_voice.py:91:34:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/evaluation/marker_tracker_loader.py#L31'>rasa/core/evaluation/marker_tracker_loader.py:31:24:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/precomputation.py#L60'>rasa/core/featurizers/precomputation.py:60:64:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -305 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/api_connexion/schemas/common_schema.py#L106'>airflow/api_connexion/schemas/common_schema.py:106:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/example_dags/plugins/listener_plugin.py#L26'>airflow/example_dags/plugins/listener_plugin.py:26:17:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/example_dags/plugins/workday.py#L103'>airflow/example_dags/plugins/workday.py:103:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L309'>airflow/operators/python.py:309:38:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L330'>airflow/operators/python.py:330:42:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/operators/python.py#L343'>airflow/operators/python.py:343:41:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L71'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:71:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L256'>airflow/providers/amazon/aws/hooks/emr.py:256:31:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L257'>airflow/providers/amazon/aws/hooks/emr.py:257:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/airflow/providers/amazon/aws/hooks/emr.py#L258'>airflow/providers/amazon/aws/hooks/emr.py:258:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 295 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/font-awesome/fontawesome_icon.py#L11'>examples/advanced/extensions/font-awesome/fontawesome_icon.py:11:24:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/widget.py#L15'>examples/advanced/extensions/widget.py:15:22:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/glyphs.py#L797'>src/bokeh/models/glyphs.py:797:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_color.py#L74'>src/bokeh/sphinxext/bokeh_color.py:74:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_enum.py#L95'>src/bokeh/sphinxext/bokeh_enum.py:95:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_example_metadata.py#L70'>src/bokeh/sphinxext/bokeh_example_metadata.py:70:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_jinja.py#L77'>src/bokeh/sphinxext/bokeh_jinja.py:77:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_model.py#L105'>src/bokeh/sphinxext/bokeh_model.py:105:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_options.py#L96'>src/bokeh/sphinxext/bokeh_options.py:96:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_plot.py#L157'>src/bokeh/sphinxext/bokeh_plot.py:157:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9ca69c5eb5c7718f9ff41be13603b17b6c384593/tools/pylint/log_checker.py#L30'>tools/pylint/log_checker.py:30:15:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
+ <a href='https://github.com/rotki/rotki/blob/9ca69c5eb5c7718f9ff41be13603b17b6c384593/tools/pylint/not_checker.py#L20'>tools/pylint/not_checker.py:20:15:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF012`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1231 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0001_initial.py#L12'>analytics/migrations/0001_initial.py:12:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0001_initial.py#L7'>analytics/migrations/0001_initial.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0002_remove_huddlecount.py#L5'>analytics/migrations/0002_remove_huddlecount.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0002_remove_huddlecount.py#L9'>analytics/migrations/0002_remove_huddlecount.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0003_fillstate.py#L5'>analytics/migrations/0003_fillstate.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0003_fillstate.py#L9'>analytics/migrations/0003_fillstate.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0004_add_subgroup.py#L5'>analytics/migrations/0004_add_subgroup.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0004_add_subgroup.py#L9'>analytics/migrations/0004_add_subgroup.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0005_alter_field_size.py#L5'>analytics/migrations/0005_alter_field_size.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0005_alter_field_size.py#L9'>analytics/migrations/0005_alter_field_size.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0006_add_subgroup_to_unique_constraints.py#L5'>analytics/migrations/0006_add_subgroup_to_unique_constraints.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0006_add_subgroup_to_unique_constraints.py#L9'>analytics/migrations/0006_add_subgroup_to_unique_constraints.py:9:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0007_remove_interval.py#L10'>analytics/migrations/0007_remove_interval.py:10:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0007_remove_interval.py#L6'>analytics/migrations/0007_remove_interval.py:6:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0008_add_count_indexes.py#L11'>analytics/migrations/0008_add_count_indexes.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0008_add_count_indexes.py#L6'>analytics/migrations/0008_add_count_indexes.py:6:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0009_remove_messages_to_stream_stat.py#L24'>analytics/migrations/0009_remove_messages_to_stream_stat.py:24:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0009_remove_messages_to_stream_stat.py#L28'>analytics/migrations/0009_remove_messages_to_stream_stat.py:28:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0010_clear_messages_sent_values.py#L24'>analytics/migrations/0010_clear_messages_sent_values.py:24:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0010_clear_messages_sent_values.py#L26'>analytics/migrations/0010_clear_messages_sent_values.py:26:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0011_clear_analytics_tables.py#L21'>analytics/migrations/0011_clear_analytics_tables.py:21:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0011_clear_analytics_tables.py#L25'>analytics/migrations/0011_clear_analytics_tables.py:25:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0012_add_on_delete.py#L12'>analytics/migrations/0012_add_on_delete.py:12:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0012_add_on_delete.py#L8'>analytics/migrations/0012_add_on_delete.py:8:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0013_remove_anomaly.py#L11'>analytics/migrations/0013_remove_anomaly.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0013_remove_anomaly.py#L7'>analytics/migrations/0013_remove_anomaly.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0014_remove_fillstate_last_modified.py#L11'>analytics/migrations/0014_remove_fillstate_last_modified.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0014_remove_fillstate_last_modified.py#L7'>analytics/migrations/0014_remove_fillstate_last_modified.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0015_clear_duplicate_counts.py#L58'>analytics/migrations/0015_clear_duplicate_counts.py:58:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0015_clear_duplicate_counts.py#L62'>analytics/migrations/0015_clear_duplicate_counts.py:62:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0016_unique_constraint_when_subgroup_null.py#L11'>analytics/migrations/0016_unique_constraint_when_subgroup_null.py:11:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0016_unique_constraint_when_subgroup_null.py#L7'>analytics/migrations/0016_unique_constraint_when_subgroup_null.py:7:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0017_regenerate_partial_indexes.py#L17'>analytics/migrations/0017_regenerate_partial_indexes.py:17:18:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/migrations/0017_regenerate_partial_indexes.py#L5'>analytics/migrations/0017_regenerate_partial_indexes.py:5:20:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L106'>analytics/models.py:106:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L120'>analytics/models.py:120:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L138'>analytics/models.py:138:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L152'>analytics/models.py:152:19:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/zulip/zulip/blob/e515574b3efa7471ec589a2f581734227ee6a9b3/analytics/models.py#L53'>analytics/models.py:53:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
... 1192 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF012 | 1563 | 0 | 1563 | 0 | 0 |
| RUF100 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @zanieb by @charliermarsh on 2023-12-19 00:56_

---

_Closed by @charliermarsh on 2024-01-26 19:34_

---

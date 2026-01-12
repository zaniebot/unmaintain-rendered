```yaml
number: 8625
title: Remove unecessary commentary in PD901 message
type: pull_request
state: merged
author: OtherBarry
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-11-12T02:07:52Z
updated_at: 2023-11-12T23:23:55Z
url: https://github.com/astral-sh/ruff/pull/8625
synced_at: 2026-01-10T23:40:55Z
```

# Remove unecessary commentary in PD901 message

---

_Pull request opened by @OtherBarry on 2023-11-12 02:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Removes unnecessary commentary from the PD901 message. This does make it different from pandas-vet, but it improves consistency with the rest of messages.

Current Message:

> `df` is a bad variable name. Be kinder to your future self.


New Message

> `df` is a bad variable name.


## Test Plan

The relevant snapshot has been updated with the new message.




---

_Comment by @github-actions[bot] on 2023-11-12 02:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+142 -142 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+52 -52 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/example_dags/tutorial_objectstorage.py#L101'>airflow/example_dags/tutorial_objectstorage.py:101:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/example_dags/tutorial_objectstorage.py#L101'>airflow/example_dags/tutorial_objectstorage.py:101:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/apache/hive/hooks/hive.py#L1047'>airflow/providers/apache/hive/hooks/hive.py:1047:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/apache/hive/hooks/hive.py#L1047'>airflow/providers/apache/hive/hooks/hive.py:1047:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/exasol/hooks/exasol.py#L86'>airflow/providers/exasol/hooks/exasol.py:86:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/exasol/hooks/exasol.py#L86'>airflow/providers/exasol/hooks/exasol.py:86:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L173'>airflow/providers/presto/hooks/presto.py:173:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L173'>airflow/providers/presto/hooks/presto.py:173:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L176'>airflow/providers/presto/hooks/presto.py:176:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L176'>airflow/providers/presto/hooks/presto.py:176:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L310'>airflow/providers/salesforce/hooks/salesforce.py:310:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L310'>airflow/providers/salesforce/hooks/salesforce.py:310:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L369'>airflow/providers/salesforce/hooks/salesforce.py:369:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L369'>airflow/providers/salesforce/hooks/salesforce.py:369:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/base_sql_to_slack.py#L82'>airflow/providers/slack/transfers/base_sql_to_slack.py:82:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/base_sql_to_slack.py#L82'>airflow/providers/slack/transfers/base_sql_to_slack.py:82:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/sql_to_slack_webhook.py#L159'>airflow/providers/slack/transfers/sql_to_slack_webhook.py:159:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/sql_to_slack_webhook.py#L159'>airflow/providers/slack/transfers/sql_to_slack_webhook.py:159:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
... 86 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+90 -90 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/brewer.py#L19'>examples/plotting/brewer.py:19:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/brewer.py#L19'>examples/plotting/brewer.py:19:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/interactive_legend.py#L23'>examples/plotting/interactive_legend.py:23:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/interactive_legend.py#L23'>examples/plotting/interactive_legend.py:23:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L27'>examples/plotting/periodic_shells.py:27:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L27'>examples/plotting/periodic_shells.py:27:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L31'>examples/plotting/periodic_shells.py:31:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L31'>examples/plotting/periodic_shells.py:31:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L32'>examples/plotting/periodic_shells.py:32:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L32'>examples/plotting/periodic_shells.py:32:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L33'>examples/plotting/periodic_shells.py:33:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 149 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD901 | 284 | 142 | 142 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+142 -142 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+52 -52 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/example_dags/tutorial_objectstorage.py#L101'>airflow/example_dags/tutorial_objectstorage.py:101:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/example_dags/tutorial_objectstorage.py#L101'>airflow/example_dags/tutorial_objectstorage.py:101:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/apache/hive/hooks/hive.py#L1047'>airflow/providers/apache/hive/hooks/hive.py:1047:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/apache/hive/hooks/hive.py#L1047'>airflow/providers/apache/hive/hooks/hive.py:1047:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/exasol/hooks/exasol.py#L86'>airflow/providers/exasol/hooks/exasol.py:86:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/exasol/hooks/exasol.py#L86'>airflow/providers/exasol/hooks/exasol.py:86:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L173'>airflow/providers/presto/hooks/presto.py:173:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L173'>airflow/providers/presto/hooks/presto.py:173:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L176'>airflow/providers/presto/hooks/presto.py:176:13:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/presto/hooks/presto.py#L176'>airflow/providers/presto/hooks/presto.py:176:13:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L310'>airflow/providers/salesforce/hooks/salesforce.py:310:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L310'>airflow/providers/salesforce/hooks/salesforce.py:310:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L369'>airflow/providers/salesforce/hooks/salesforce.py:369:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/salesforce/hooks/salesforce.py#L369'>airflow/providers/salesforce/hooks/salesforce.py:369:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/base_sql_to_slack.py#L82'>airflow/providers/slack/transfers/base_sql_to_slack.py:82:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/base_sql_to_slack.py#L82'>airflow/providers/slack/transfers/base_sql_to_slack.py:82:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/sql_to_slack_webhook.py#L159'>airflow/providers/slack/transfers/sql_to_slack_webhook.py:159:9:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/apache/airflow/blob/5a6dcfd8655c9622f3838a0e66948dc3091afccb/airflow/providers/slack/transfers/sql_to_slack_webhook.py#L159'>airflow/providers/slack/transfers/sql_to_slack_webhook.py:159:9:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
... 86 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+90 -90 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L21'>examples/basic/annotations/band.py:21:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/band.py#L25'>examples/basic/annotations/band.py:25:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/annotations/polygon_annotation.py#L12'>examples/basic/annotations/polygon_annotation.py:12:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/areas/stacked_area.py#L16'>examples/basic/areas/stacked_area.py:16:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/axes/datetime_axis.py#L6'>examples/basic/axes/datetime_axis.py:6:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/basic/bars/intervals.py#L14'>examples/basic/bars/intervals.py:14:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_hide.py#L22'>examples/interaction/legends/legend_hide.py:22:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/interaction/legends/legend_mute.py#L11'>examples/interaction/legends/legend_mute.py:11:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/daylight.py#L24'>examples/models/daylight.py:24:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/trail.py#L36'>examples/models/trail.py:36:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/brewer.py#L19'>examples/plotting/brewer.py:19:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/brewer.py#L19'>examples/plotting/brewer.py:19:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/interactive_legend.py#L23'>examples/plotting/interactive_legend.py:23:5:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/interactive_legend.py#L23'>examples/plotting/interactive_legend.py:23:5:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L27'>examples/plotting/periodic_shells.py:27:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L27'>examples/plotting/periodic_shells.py:27:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L31'>examples/plotting/periodic_shells.py:31:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L31'>examples/plotting/periodic_shells.py:31:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L32'>examples/plotting/periodic_shells.py:32:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L32'>examples/plotting/periodic_shells.py:32:1:</a> PD901 `df` is a bad variable name. Be kinder to your future self.
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/plotting/periodic_shells.py#L33'>examples/plotting/periodic_shells.py:33:1:</a> PD901 Avoid using the generic variable name `df` for DataFrames
... 149 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD901 | 284 | 142 | 142 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2023-11-12 17:13_

Thanks, good call. I tweaked it even further to make it more consistent.

---

_Label `documentation` added by @charliermarsh on 2023-11-12 17:13_

---

_Merged by @charliermarsh on 2023-11-12 17:20_

---

_Closed by @charliermarsh on 2023-11-12 17:20_

---

_Branch deleted on 2023-11-12 23:23_

---

```yaml
number: 14068
title: Improve some rule messages and docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2024-11-03T19:20:53Z
updated_at: 2024-11-04T03:21:30Z
url: https://github.com/astral-sh/ruff/pull/14068
synced_at: 2026-01-12T15:55:46Z
```

# Improve some rule messages and docs

---

_@charliermarsh_

_No description provided._

---

_Label `docstring` added by @charliermarsh on 2024-11-03 19:20_

---

_Marked ready for review by @charliermarsh on 2024-11-03 19:21_

---

_Label `docstring` removed by @charliermarsh on 2024-11-03 19:21_

---

_Label `documentation` added by @charliermarsh on 2024-11-03 19:21_

---

_Merged by @charliermarsh on 2024-11-03 19:25_

---

_Closed by @charliermarsh on 2024-11-03 19:25_

---

_Branch deleted on 2024-11-03 19:25_

---

_Comment by @github-actions[bot] on 2024-11-03 19:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+111 -111 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/modal.py#L236'>disnake/ui/modal.py:236:22:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/modal.py#L236'>disnake/ui/modal.py:236:22:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/view.py#L425'>disnake/ui/view.py:425:35:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/view.py#L425'>disnake/ui/view.py:425:35:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/view.py#L530'>disnake/ui/view.py:530:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/view.py#L530'>disnake/ui/view.py:530:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L345'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py:345:25:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L345'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py:345:25:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+84 -84 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L651'>airflow/configuration.py:651:42:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L651'>airflow/configuration.py:651:42:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L679'>airflow/configuration.py:679:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L679'>airflow/configuration.py:679:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L714'>airflow/configuration.py:714:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L714'>airflow/configuration.py:714:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L916'>airflow/configuration.py:916:69:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/configuration.py#L916'>airflow/configuration.py:916:69:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L127'>airflow/jobs/scheduler_job_runner.py:127:39:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L127'>airflow/jobs/scheduler_job_runner.py:127:39:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L128'>airflow/jobs/scheduler_job_runner.py:128:46:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L128'>airflow/jobs/scheduler_job_runner.py:128:46:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L1600'>airflow/jobs/scheduler_job_runner.py:1600:47:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L1600'>airflow/jobs/scheduler_job_runner.py:1600:47:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L1634'>airflow/jobs/scheduler_job_runner.py:1634:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L1634'>airflow/jobs/scheduler_job_runner.py:1634:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L506'>airflow/jobs/scheduler_job_runner.py:506:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L506'>airflow/jobs/scheduler_job_runner.py:506:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L526'>airflow/jobs/scheduler_job_runner.py:526:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L526'>airflow/jobs/scheduler_job_runner.py:526:29:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L566'>airflow/jobs/scheduler_job_runner.py:566:54:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L566'>airflow/jobs/scheduler_job_runner.py:566:54:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L568'>airflow/jobs/scheduler_job_runner.py:568:21:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/jobs/scheduler_job_runner.py#L568'>airflow/jobs/scheduler_job_runner.py:568:21:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L2915'>airflow/models/taskinstance.py:2915:39:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L2915'>airflow/models/taskinstance.py:2915:39:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3594'>airflow/models/taskinstance.py:3594:28:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3594'>airflow/models/taskinstance.py:3594:28:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3595'>airflow/models/taskinstance.py:3595:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3595'>airflow/models/taskinstance.py:3595:30:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3601'>airflow/models/taskinstance.py:3601:49:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3601'>airflow/models/taskinstance.py:3601:49:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3602'>airflow/models/taskinstance.py:3602:53:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/models/taskinstance.py#L3602'>airflow/models/taskinstance.py:3602:53:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/helm_tests/other/test_redis.py#L61'>helm_tests/other/test_redis.py:61:41:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/helm_tests/other/test_redis.py#L61'>helm_tests/other/test_redis.py:61:41:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/providers/src/airflow/providers/apache/hive/operators/hive_stats.py#L101'>providers/src/airflow/providers/apache/hive/operators/hive_stats.py:101:17:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
... 131 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/scripts/permissions_cleanup.py#L32'>scripts/permissions_cleanup.py:32:19:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/scripts/permissions_cleanup.py#L32'>scripts/permissions_cleanup.py:32:19:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/models/dashboard.py#L272'>superset/models/dashboard.py:272:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/models/dashboard.py#L272'>superset/models/dashboard.py:272:34:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/utils/decorators.py#L158'>superset/utils/decorators.py:158:9:</a> RUF034 Useless `if`-`else` condition
- <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/utils/decorators.py#L158'>superset/utils/decorators.py:158:9:</a> RUF034 Useless if-else condition
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/viz.py#L1447'>superset/viz.py:1447:20:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/viz.py#L1447'>superset/viz.py:1447:20:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
... 5 additional changes omitted for rule RUF031
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/graph/node_and_edge_attributes.py#L21'>examples/topics/graph/node_and_edge_attributes.py:21:16:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/graph/node_and_edge_attributes.py#L21'>examples/topics/graph/node_and_edge_attributes.py:21:16:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/unemployment.py#L82'>src/bokeh/sampledata/unemployment.py:82:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/unemployment.py#L82'>src/bokeh/sampledata/unemployment.py:82:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/us_counties.py#L114'>src/bokeh/sampledata/us_counties.py:114:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/us_counties.py#L114'>src/bokeh/sampledata/us_counties.py:114:18:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_util__colors.py#L141'>tests/unit/bokeh/colors/test_util__colors.py:141:24:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_util__colors.py#L141'>tests/unit/bokeh/colors/test_util__colors.py:141:24:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/9a015d19514597ad5d1c31f566a0a2bdb17d4cc5/pandas/tests/io/formats/style/test_style.py#L934'>pandas/tests/io/formats/style/test_style.py:934:26:</a> RUF034 Useless `if`-`else` condition
- <a href='https://github.com/pandas-dev/pandas/blob/9a015d19514597ad5d1c31f566a0a2bdb17d4cc5/pandas/tests/io/formats/style/test_style.py#L934'>pandas/tests/io/formats/style/test_style.py:934:26:</a> RUF034 Useless if-else condition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+12 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/analytics/lib/counts.py#L541'>analytics/lib/counts.py:541:28:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/analytics/lib/counts.py#L541'>analytics/lib/counts.py:541:28:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/actions/message_edit.py#L968'>zerver/actions/message_edit.py:968:17:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/actions/message_edit.py#L968'>zerver/actions/message_edit.py:968:17:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/email_notifications.py#L646'>zerver/lib/email_notifications.py:646:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/email_notifications.py#L646'>zerver/lib/email_notifications.py:646:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/email_notifications.py#L648'>zerver/lib/email_notifications.py:648:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/email_notifications.py#L648'>zerver/lib/email_notifications.py:648:32:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/mention.py#L133'>zerver/lib/mention.py:133:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/mention.py#L133'>zerver/lib/mention.py:133:33:</a> RUF031 [*] Avoid parentheses for tuples in subscripts.
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF031 | 218 | 109 | 109 | 0 | 0 |
| RUF034 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Label `preview` added by @dhruvmanila on 2024-11-04 03:21_

---

_Comment by @dhruvmanila on 2024-11-04 03:21_

I've added the https://github.com/astral-sh/ruff/labels/preview label as all affected rules are in preview (per ecosystem comment).

---

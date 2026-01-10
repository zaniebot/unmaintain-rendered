```yaml
number: 9627
title: "[`flake8-todo`] - Align `TD003` with the VSCode Github PR extension"
type: pull_request
state: closed
author: MartinBernstorff
labels:
  - rule
assignees: []
base: main
head: mbern/8061/td003-support-vscode-github-syntax
created_at: 2024-01-23T18:10:52Z
updated_at: 2025-01-16T07:26:49Z
url: https://github.com/astral-sh/ruff/pull/9627
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-todo`] - Align `TD003` with the VSCode Github PR extension

---

_Pull request opened by @MartinBernstorff on 2024-01-23 18:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The [GitHub VSCode extension](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) supports automatically creating an issue from a comment with two formats, selectable under the setting `githubIssues.createInsertFormat`.

```
# TODO: https://github.com/astral-sh/ruff/issues/8061 Todo description
# TODO: #8061 Todo description
```

As discussed in #8061, this PR expands `TD003` to consider those two formats valid.

## Design considerations

Since the new behaviour is single-line analysis, I considered adding it to the `static_errors` fn, but decided co-locating all the `TD003` code instead made more sense.

## Test Plan

The two formats were added to `TD003.py` and did not raise errors. I'm somewhat concerned that the new regex patterns could induce false negatives, and would love reviewer feedback on that!


---

_Renamed from "Align `TD003` with the VSCode Github PR extension" to "[`flake8-todo`] Align `TD003` with the VSCode Github PR extension" by @MartinBernstorff on 2024-01-23 18:39_

---

_Renamed from "[`flake8-todo`] Align `TD003` with the VSCode Github PR extension" to "[`flake8-todo`] - Align `TD003` with the VSCode Github PR extension" by @MartinBernstorff on 2024-01-23 18:39_

---

_Comment by @github-actions[bot] on 2024-01-23 18:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+678 -698 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+275 -276 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/api_connexion/schemas/dag_schema.py#L100'>airflow/api_connexion/schemas/dag_schema.py:100:55:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/api_connexion/schemas/dag_schema.py#L100'>airflow/api_connexion/schemas/dag_schema.py:100:55:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L239'>airflow/cli/cli_config.py:239:6:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L239'>airflow/cli/cli_config.py:239:6:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L579'>airflow/cli/cli_config.py:579:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L579'>airflow/cli/cli_config.py:579:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L595'>airflow/cli/cli_config.py:595:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L595'>airflow/cli/cli_config.py:595:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L191'>airflow/cli/commands/task_command.py:191:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L191'>airflow/cli/commands/task_command.py:191:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L237'>airflow/cli/commands/task_command.py:237:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L237'>airflow/cli/commands/task_command.py:237:15:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L472'>airflow/cli/commands/task_command.py:472:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L472'>airflow/cli/commands/task_command.py:472:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L689'>airflow/cli/commands/task_command.py:689:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L689'>airflow/cli/commands/task_command.py:689:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/compat/functools.pyi#L20'>airflow/compat/functools.pyi:20:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/compat/functools.pyi#L20'>airflow/compat/functools.pyi:20:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/dag_processing/manager.py#L1293'>airflow/dag_processing/manager.py:1293:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/dag_processing/manager.py#L1293'>airflow/dag_processing/manager.py:1293:7:</a> TD003 Missing issue link on the line following this TODO
... 531 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+180 -198 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/layout/plot_fixed_frame_size.py#L38'>examples/integration/layout/plot_fixed_frame_size.py:38:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/layout/plot_fixed_frame_size.py#L38'>examples/integration/layout/plot_fixed_frame_size.py:38:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/legends.py#L53'>examples/models/legends.py:53:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/legends.py#L53'>examples/models/legends.py:53:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L50'>examples/server/app/simple_hdf5/main.py:50:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L50'>examples/server/app/simple_hdf5/main.py:50:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L77'>release/credentials.py:77:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L77'>release/credentials.py:77:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L84'>release/credentials.py:84:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L84'>release/credentials.py:84:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L187'>src/bokeh/application/application.py:187:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L187'>src/bokeh/application/application.py:187:15:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L168'>src/bokeh/application/handlers/code.py:168:11:</a> TD003 Missing issue link for this TODO
... 365 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+223 -224 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L122'>analytics/lib/counts.py:122:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L122'>analytics/lib/counts.py:122:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L266'>analytics/lib/counts.py:266:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L266'>analytics/lib/counts.py:266:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/management/commands/populate_analytics_db.py#L74'>analytics/management/commands/populate_analytics_db.py:74:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/management/commands/populate_analytics_db.py#L74'>analytics/management/commands/populate_analytics_db.py:74:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L111'>analytics/views/stats.py:111:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L111'>analytics/views/stats.py:111:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L451'>analytics/views/stats.py:451:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L451'>analytics/views/stats.py:451:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1343'>corporate/lib/stripe.py:1343:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1343'>corporate/lib/stripe.py:1343:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1923'>corporate/lib/stripe.py:1923:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1923'>corporate/lib/stripe.py:1923:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L2190'>corporate/lib/stripe.py:2190:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L2190'>corporate/lib/stripe.py:2190:15:</a> TD003 Missing issue link on the line following this TODO
... 431 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 1376 | 678 | 698 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+678 -698 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+275 -276 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/api_connexion/schemas/dag_schema.py#L100'>airflow/api_connexion/schemas/dag_schema.py:100:55:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/api_connexion/schemas/dag_schema.py#L100'>airflow/api_connexion/schemas/dag_schema.py:100:55:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L239'>airflow/cli/cli_config.py:239:6:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L239'>airflow/cli/cli_config.py:239:6:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L579'>airflow/cli/cli_config.py:579:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L579'>airflow/cli/cli_config.py:579:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L595'>airflow/cli/cli_config.py:595:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/cli_config.py#L595'>airflow/cli/cli_config.py:595:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L191'>airflow/cli/commands/task_command.py:191:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L191'>airflow/cli/commands/task_command.py:191:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L237'>airflow/cli/commands/task_command.py:237:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L237'>airflow/cli/commands/task_command.py:237:15:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L472'>airflow/cli/commands/task_command.py:472:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L472'>airflow/cli/commands/task_command.py:472:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L689'>airflow/cli/commands/task_command.py:689:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/cli/commands/task_command.py#L689'>airflow/cli/commands/task_command.py:689:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/compat/functools.pyi#L20'>airflow/compat/functools.pyi:20:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/compat/functools.pyi#L20'>airflow/compat/functools.pyi:20:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/dag_processing/manager.py#L1293'>airflow/dag_processing/manager.py:1293:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/fee7e33983a0c3f9eeb1f72469d8a68fbb81fa6c/airflow/dag_processing/manager.py#L1293'>airflow/dag_processing/manager.py:1293:7:</a> TD003 Missing issue link on the line following this TODO
... 531 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+180 -198 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/layout/plot_fixed_frame_size.py#L38'>examples/integration/layout/plot_fixed_frame_size.py:38:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/layout/plot_fixed_frame_size.py#L38'>examples/integration/layout/plot_fixed_frame_size.py:38:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/legends.py#L53'>examples/models/legends.py:53:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/legends.py#L53'>examples/models/legends.py:53:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L50'>examples/server/app/simple_hdf5/main.py:50:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L50'>examples/server/app/simple_hdf5/main.py:50:3:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L77'>release/credentials.py:77:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L77'>release/credentials.py:77:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L84'>release/credentials.py:84:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L84'>release/credentials.py:84:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L187'>src/bokeh/application/application.py:187:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L187'>src/bokeh/application/application.py:187:15:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L168'>src/bokeh/application/handlers/code.py:168:11:</a> TD003 Missing issue link for this TODO
... 365 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+223 -224 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L122'>analytics/lib/counts.py:122:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L122'>analytics/lib/counts.py:122:7:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L266'>analytics/lib/counts.py:266:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/lib/counts.py#L266'>analytics/lib/counts.py:266:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/management/commands/populate_analytics_db.py#L74'>analytics/management/commands/populate_analytics_db.py:74:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/management/commands/populate_analytics_db.py#L74'>analytics/management/commands/populate_analytics_db.py:74:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L111'>analytics/views/stats.py:111:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L111'>analytics/views/stats.py:111:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L451'>analytics/views/stats.py:451:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/analytics/views/stats.py#L451'>analytics/views/stats.py:451:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1343'>corporate/lib/stripe.py:1343:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1343'>corporate/lib/stripe.py:1343:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1923'>corporate/lib/stripe.py:1923:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L1923'>corporate/lib/stripe.py:1923:11:</a> TD003 Missing issue link on the line following this TODO
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L2190'>corporate/lib/stripe.py:2190:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/corporate/lib/stripe.py#L2190'>corporate/lib/stripe.py:2190:15:</a> TD003 Missing issue link on the line following this TODO
... 431 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 1376 | 678 | 698 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:233 on 2024-01-24 14:42_

Is it correct that the reason you removed the `\s` from the "issue link" regex is so that the link can be anywhere in the comment?

Also, can we keep the asterisk for `\s*` so that there can be any number of whitespace instead of just one? Is there a reason you removed them?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_todos/snapshots/ruff_linter__rules__flake8_todos__tests__missing-todo-link_TD003.py.snap`:49 on 2024-01-24 14:45_

Can you add a test case for `#002` variant as well?

---

_@dhruvmanila reviewed on 2024-01-24 14:53_

Thanks for putting together this PR!

It seems that there are lot of violations which got removed from the ecosystem checks. For example, https://github.com/apache/airflow/blob/c0f76013917ee57b3cc2cebcf08e4421143eefc7/airflow/dag_processing/manager.py#L1293 isn't being triggered now. I think the issue might be the re-ordering of the code as there's an issue link after a couple of lines of the TODO in the source code.

---

_Label `rule` added by @dhruvmanila on 2024-01-24 14:54_

---

_Review comment by @MartinBernstorff on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:233 on 2024-01-26 18:09_

> Is it correct that the reason you removed the \s from the "issue link" regex is so that the link can be anywhere in the comment?

Yep, absolutely! 

> Also, can we keep the asterisk for \s* so that there can be any number of whitespace instead of just one? Is there a reason you removed them?

I removed 'em because I'd consider `#     TODO` incorrect, but if we were to enforce that, it should be through TD007. I've re-added them.

---

_Review comment by @MartinBernstorff on `crates/ruff_linter/src/rules/flake8_todos/snapshots/ruff_linter__rules__flake8_todos__tests__missing-todo-link_TD003.py.snap`:49 on 2024-01-26 18:10_

Yep! It's at the bottom of TD003.py, but doesn't error, so is not in the `.snap`. The link-case is included due to being within the context of the non-linnk failing example :-)

---

_@MartinBernstorff reviewed on 2024-01-26 18:10_

---

_Comment by @MartinBernstorff on 2024-01-26 18:19_

> Thanks for putting together this PR!
> 
> It seems that there are lot of violations which got removed from the ecosystem checks. For example, https://github.com/apache/airflow/blob/c0f76013917ee57b3cc2cebcf08e4421143eefc7/airflow/dag_processing/manager.py#L1293 isn't being triggered now. I think the issue might be the re-ordering of the code as there's an issue link after a couple of lines of the TODO in the source code.

This is a really good point! I'm implementing a solution that should make the changes smaller in the ecosystem.

That said, I think this opens up a discussion about desirable behaviour. E.g. the linked comment does, in fact, contain an issue link. It just doesn't occur on the same or the next line. Would we want to support that, or is the false-negative rate perhaps too high?

I'd lean towards "on same or next line", but would love to hear your thoughts!

---

_Comment by @codspeed-hq[bot] on 2024-01-26 20:04_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/MartinBernstorff:mbern/8061/td003-support-vscode-github-syntax)

### Merging #9627 will **not alter performance**

<sub>Comparing <code>MartinBernstorff:mbern/8061/td003-support-vscode-github-syntax</code> (18df98a) with <code>main</code> (45f01e7)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @MartinBernstorff on 2024-01-26 20:14_

The large amount of ecosystem changes in the previous version was largely due to changing the regex set. 

I've had to split into two regex sets to keep the ecosystem changes small.

Unsure if that should give such a large change to performance, would love some input here.

Let me know what you think!

---

_Review requested from @dhruvmanila by @MartinBernstorff on 2024-01-31 09:51_

---

_Comment by @dhruvmanila on 2024-02-07 16:25_

> I'd lean towards "on same or next line", but would love to hear your thoughts!

Yeah, I'm thinking in the same direction. It seems like the extension only provides the code action to create the issue if the text / link isn't on the same line. So, your intuition to making it work for same or next line should reduce the false negative rate.

---

_Comment by @simonpercivall on 2024-09-28 09:42_

This seems to have gotten a bit stale, but it would still be very nice to get this. Can I assist in any way to help push this forward?

---

_Comment by @MartinBernstorff on 2024-09-30 11:45_

Hi @simonpercivall! Absolutely, it's on me for not finishing this. I'm not sure I ever will, so from my end you're free to fork/replace this PR 👍 

---

_Comment by @dhruvmanila on 2024-10-01 03:34_

(I've merged the latest `main` in this PR, @simonpercivall if you're interested feel free to work on top of this or start a new, whatever seems easier for you.)

---

_Comment by @dylwil3 on 2025-01-15 23:24_

Gonna move this work to #15519 so that we can work off of main. (I tried rebasing first but was not confident in some of the messier conflict resolutions, so it seemed cleaner to do it this way). @MartinBernstorff thank you for the contribution, and sorry for the long delay in resolution!

---

_Closed by @dylwil3 on 2025-01-15 23:24_

---

_Comment by @MartinBernstorff on 2025-01-16 07:26_

@dylwil3 Absolutely _no_ worries, I didn't follow up either, and super happy to see this merged!

---

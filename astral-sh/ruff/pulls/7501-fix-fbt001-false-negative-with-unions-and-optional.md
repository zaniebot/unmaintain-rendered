```yaml
number: 7501
title: Fix FBT001 false negative with unions and optional
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-fbt001-false-negative-union-of-bool
created_at: 2023-09-18T20:17:57Z
updated_at: 2023-11-12T20:09:51Z
url: https://github.com/astral-sh/ruff/pull/7501
synced_at: 2026-01-10T23:40:55Z
```

# Fix FBT001 false negative with unions and optional

---

_Pull request opened by @JonathanPlasse on 2023-09-18 20:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Close #7487

In the spirit of `flake8-boolean-trap`, any positional argument that can accept a boolean should raise `FBT001`.
Raise `FBT001` for all annotations that accept booleans (e.g. `Optional[bool]`, `Union[int, bool]`).

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add a fixture, with an annotation using `|`, `Optional`, and `Union`, and containing a boolean.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-09-18 20:35_

## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+214 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+89 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L120'>airflow/callbacks/callback_requests.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L80'>airflow/callbacks/callback_requests.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_external_python.py#L36'>airflow/decorators/branch_external_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_python.py#L36'>airflow/decorators/branch_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_virtualenv.py#L36'>airflow/decorators/branch_virtualenv.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/external_python.py#L38'>airflow/decorators/external_python.py:38:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python.py#L63'>airflow/decorators/python.py:63:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python_virtualenv.py#L37'>airflow/decorators/python_virtualenv.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/short_circuit.py#L37'>airflow/decorators/short_circuit.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/lineage/entities.py#L73'>airflow/lineage/entities.py:73:21:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/baseoperator.py#L1482'>airflow/models/baseoperator.py:1482:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L2869'>airflow/models/dag.py:2869:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L3836'>airflow/models/dag.py:3836:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L460'>airflow/models/dag.py:460:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L101'>airflow/models/dagbag.py:101:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L98'>airflow/models/dagbag.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L99'>airflow/models/dagbag.py:99:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L212'>airflow/models/dagrun.py:212:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L379'>airflow/models/dagrun.py:379:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/taskinstance.py#L2599'>airflow/models/taskinstance.py:2599:9:</a> FBT001 Boolean-typed positional argument in function definition
... 69 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/embed/util.py#L183'>src/bokeh/embed/util.py:183:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/server/tornado.py#L608'>src/bokeh/server/tornado.py:608:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/settings.py#L167'>src/bokeh/settings.py:167:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:41:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/lib/counts.py#L301'>analytics/lib/counts.py:301:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/views/support.py#L169'>analytics/views/support.py:169:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/corporate/tests/test_stripe.py#L448'>corporate/tests/test_stripe.py:448:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/create_user.py#L413'>zerver/actions/create_user.py:413:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_settings.py#L400'>zerver/actions/user_settings.py:400:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_status.py#L11'>zerver/actions/user_status.py:11:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L159'>zerver/lib/create_user.py:159:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L164'>zerver/lib/create_user.py:164:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/event_schema.py#L1572'>zerver/lib/event_schema.py:1572:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L1931'>zerver/lib/export.py:1931:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L2392'>zerver/lib/export.py:2392:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/management.py#L120'>zerver/lib/management.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1728'>zerver/lib/message.py:1728:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1753'>zerver/lib/message.py:1753:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1775'>zerver/lib/message.py:1775:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/notification_data.py#L266'>zerver/lib/notification_data.py:266:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/streams.py#L94'>zerver/lib/streams.py:94:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L1345'>zerver/lib/test_classes.py:1345:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L825'>zerver/lib/test_classes.py:825:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_runner.py#L188'>zerver/lib/test_runner.py:188:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/webhooks/git.py#L98'>zerver/lib/webhooks/git.py:98:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tests/test_realm_export.py#L124'>zerver/tests/test_realm_export.py:124:13:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L21'>zerver/views/events_register.py:21:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L46'>zerver/views/events_register.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L50'>zerver/views/events_register.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L101'>zerver/views/realm.py:101:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L112'>zerver/views/realm.py:112:5:</a> FBT001 Boolean-typed positional argument in function definition
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 214 | 214 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+209 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+84 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L120'>airflow/callbacks/callback_requests.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L80'>airflow/callbacks/callback_requests.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_external_python.py#L36'>airflow/decorators/branch_external_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_python.py#L36'>airflow/decorators/branch_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_virtualenv.py#L36'>airflow/decorators/branch_virtualenv.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/external_python.py#L38'>airflow/decorators/external_python.py:38:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python.py#L63'>airflow/decorators/python.py:63:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python_virtualenv.py#L37'>airflow/decorators/python_virtualenv.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/short_circuit.py#L37'>airflow/decorators/short_circuit.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/lineage/entities.py#L73'>airflow/lineage/entities.py:73:21:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/baseoperator.py#L1482'>airflow/models/baseoperator.py:1482:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L2869'>airflow/models/dag.py:2869:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L3836'>airflow/models/dag.py:3836:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L460'>airflow/models/dag.py:460:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L101'>airflow/models/dagbag.py:101:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L98'>airflow/models/dagbag.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L99'>airflow/models/dagbag.py:99:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L212'>airflow/models/dagrun.py:212:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L379'>airflow/models/dagrun.py:379:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/taskinstance.py#L2599'>airflow/models/taskinstance.py:2599:9:</a> FBT001 Boolean-typed positional argument in function definition
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/embed/util.py#L183'>src/bokeh/embed/util.py:183:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/server/tornado.py#L608'>src/bokeh/server/tornado.py:608:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/settings.py#L167'>src/bokeh/settings.py:167:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:41:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/lib/counts.py#L301'>analytics/lib/counts.py:301:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/views/support.py#L169'>analytics/views/support.py:169:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/corporate/tests/test_stripe.py#L448'>corporate/tests/test_stripe.py:448:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/create_user.py#L413'>zerver/actions/create_user.py:413:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_settings.py#L400'>zerver/actions/user_settings.py:400:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_status.py#L11'>zerver/actions/user_status.py:11:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L159'>zerver/lib/create_user.py:159:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L164'>zerver/lib/create_user.py:164:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/event_schema.py#L1572'>zerver/lib/event_schema.py:1572:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L1931'>zerver/lib/export.py:1931:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L2392'>zerver/lib/export.py:2392:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/management.py#L120'>zerver/lib/management.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1728'>zerver/lib/message.py:1728:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1753'>zerver/lib/message.py:1753:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1775'>zerver/lib/message.py:1775:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/notification_data.py#L266'>zerver/lib/notification_data.py:266:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/streams.py#L94'>zerver/lib/streams.py:94:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L1345'>zerver/lib/test_classes.py:1345:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L825'>zerver/lib/test_classes.py:825:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_runner.py#L188'>zerver/lib/test_runner.py:188:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/webhooks/git.py#L98'>zerver/lib/webhooks/git.py:98:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tests/test_realm_export.py#L124'>zerver/tests/test_realm_export.py:124:13:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L21'>zerver/views/events_register.py:21:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L46'>zerver/views/events_register.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L50'>zerver/views/events_register.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L101'>zerver/views/realm.py:101:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L112'>zerver/views/realm.py:112:5:</a> FBT001 Boolean-typed positional argument in function definition
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 209 | 209 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @JonathanPlasse on 2023-09-18 20:49_

This could introduce a lot of new positives.
Is it possible to have a preview of rules behavior change?

---

_Label `preview` added by @charliermarsh on 2023-09-19 12:37_

---

_@dhruvmanila reviewed on 2023-10-30 07:15_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_boolean_trap/rules/boolean_type_hint_positional_argument.rs`:107 on 2023-10-30 07:15_

With https://github.com/astral-sh/ruff/pull/8064 merged, you'll need to update this to use `Expr::StringLiteral` instead:

```suggestion
        Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) => value == "bool",
```

---

_Comment by @github-actions[bot] on 2023-11-02 09:32_

## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+214 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+89 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L120'>airflow/callbacks/callback_requests.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L80'>airflow/callbacks/callback_requests.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_external_python.py#L36'>airflow/decorators/branch_external_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_python.py#L36'>airflow/decorators/branch_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_virtualenv.py#L36'>airflow/decorators/branch_virtualenv.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/external_python.py#L38'>airflow/decorators/external_python.py:38:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python.py#L63'>airflow/decorators/python.py:63:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python_virtualenv.py#L37'>airflow/decorators/python_virtualenv.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/short_circuit.py#L37'>airflow/decorators/short_circuit.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/lineage/entities.py#L73'>airflow/lineage/entities.py:73:21:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/baseoperator.py#L1482'>airflow/models/baseoperator.py:1482:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L2869'>airflow/models/dag.py:2869:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L3836'>airflow/models/dag.py:3836:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L460'>airflow/models/dag.py:460:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L101'>airflow/models/dagbag.py:101:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L98'>airflow/models/dagbag.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L99'>airflow/models/dagbag.py:99:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L212'>airflow/models/dagrun.py:212:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L379'>airflow/models/dagrun.py:379:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/taskinstance.py#L2599'>airflow/models/taskinstance.py:2599:9:</a> FBT001 Boolean-typed positional argument in function definition
... 69 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/embed/util.py#L183'>src/bokeh/embed/util.py:183:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/server/tornado.py#L608'>src/bokeh/server/tornado.py:608:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/settings.py#L167'>src/bokeh/settings.py:167:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:41:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/lib/counts.py#L301'>analytics/lib/counts.py:301:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/views/support.py#L169'>analytics/views/support.py:169:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/corporate/tests/test_stripe.py#L448'>corporate/tests/test_stripe.py:448:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/create_user.py#L413'>zerver/actions/create_user.py:413:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_settings.py#L400'>zerver/actions/user_settings.py:400:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_status.py#L11'>zerver/actions/user_status.py:11:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L159'>zerver/lib/create_user.py:159:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L164'>zerver/lib/create_user.py:164:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/event_schema.py#L1572'>zerver/lib/event_schema.py:1572:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L1931'>zerver/lib/export.py:1931:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L2392'>zerver/lib/export.py:2392:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/management.py#L120'>zerver/lib/management.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1728'>zerver/lib/message.py:1728:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1753'>zerver/lib/message.py:1753:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1775'>zerver/lib/message.py:1775:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/notification_data.py#L266'>zerver/lib/notification_data.py:266:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/streams.py#L94'>zerver/lib/streams.py:94:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L1345'>zerver/lib/test_classes.py:1345:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L825'>zerver/lib/test_classes.py:825:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_runner.py#L188'>zerver/lib/test_runner.py:188:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/webhooks/git.py#L98'>zerver/lib/webhooks/git.py:98:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tests/test_realm_export.py#L124'>zerver/tests/test_realm_export.py:124:13:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L21'>zerver/views/events_register.py:21:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L46'>zerver/views/events_register.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L50'>zerver/views/events_register.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L101'>zerver/views/realm.py:101:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L112'>zerver/views/realm.py:112:5:</a> FBT001 Boolean-typed positional argument in function definition
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 214 | 214 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+209 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+84 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L120'>airflow/callbacks/callback_requests.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/callbacks/callback_requests.py#L80'>airflow/callbacks/callback_requests.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_external_python.py#L36'>airflow/decorators/branch_external_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_python.py#L36'>airflow/decorators/branch_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/branch_virtualenv.py#L36'>airflow/decorators/branch_virtualenv.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/external_python.py#L38'>airflow/decorators/external_python.py:38:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python.py#L63'>airflow/decorators/python.py:63:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/python_virtualenv.py#L37'>airflow/decorators/python_virtualenv.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/decorators/short_circuit.py#L37'>airflow/decorators/short_circuit.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/lineage/entities.py#L73'>airflow/lineage/entities.py:73:21:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/baseoperator.py#L1482'>airflow/models/baseoperator.py:1482:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L2869'>airflow/models/dag.py:2869:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L3836'>airflow/models/dag.py:3836:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dag.py#L460'>airflow/models/dag.py:460:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L101'>airflow/models/dagbag.py:101:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L98'>airflow/models/dagbag.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagbag.py#L99'>airflow/models/dagbag.py:99:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L212'>airflow/models/dagrun.py:212:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/dagrun.py#L379'>airflow/models/dagrun.py:379:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/283fb9fd317862e5b375dbcc126a660fe8a22e11/airflow/models/taskinstance.py#L2599'>airflow/models/taskinstance.py:2599:9:</a> FBT001 Boolean-typed positional argument in function definition
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/embed/util.py#L183'>src/bokeh/embed/util.py:183:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/server/tornado.py#L608'>src/bokeh/server/tornado.py:608:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/settings.py#L167'>src/bokeh/settings.py:167:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:41:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/lib/counts.py#L301'>analytics/lib/counts.py:301:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/analytics/views/support.py#L169'>analytics/views/support.py:169:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/corporate/tests/test_stripe.py#L448'>corporate/tests/test_stripe.py:448:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/create_user.py#L413'>zerver/actions/create_user.py:413:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_settings.py#L400'>zerver/actions/user_settings.py:400:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/actions/user_status.py#L11'>zerver/actions/user_status.py:11:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L159'>zerver/lib/create_user.py:159:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/create_user.py#L164'>zerver/lib/create_user.py:164:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/event_schema.py#L1572'>zerver/lib/event_schema.py:1572:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L1931'>zerver/lib/export.py:1931:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/export.py#L2392'>zerver/lib/export.py:2392:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/management.py#L120'>zerver/lib/management.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1728'>zerver/lib/message.py:1728:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1753'>zerver/lib/message.py:1753:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/message.py#L1775'>zerver/lib/message.py:1775:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/notification_data.py#L266'>zerver/lib/notification_data.py:266:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/streams.py#L94'>zerver/lib/streams.py:94:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L1345'>zerver/lib/test_classes.py:1345:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_classes.py#L825'>zerver/lib/test_classes.py:825:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/test_runner.py#L188'>zerver/lib/test_runner.py:188:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/lib/webhooks/git.py#L98'>zerver/lib/webhooks/git.py:98:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tests/test_realm_export.py#L124'>zerver/tests/test_realm_export.py:124:13:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L21'>zerver/views/events_register.py:21:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L46'>zerver/views/events_register.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/events_register.py#L50'>zerver/views/events_register.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L101'>zerver/views/realm.py:101:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5dc9b060d2f14d2224b4ed4497605663a2c33e77/zerver/views/realm.py#L112'>zerver/views/realm.py:112:5:</a> FBT001 Boolean-typed positional argument in function definition
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 209 | 209 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @JonathanPlasse on 2023-11-02 10:19_

How should I proceed to make this behavior change only for the preview mode?

---

_Comment by @charliermarsh on 2023-11-06 00:51_

@JonathanPlasse - I think you could change `match_annotation_to_bool` to take the preview flag as argument, and fork internally?

---

_@charliermarsh reviewed on 2023-11-07 22:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_boolean_trap/rules/boolean_type_hint_positional_argument.rs`:127 on 2023-11-07 22:23_

@dhruvmanila - Is it possible to use `TypingTarget` for this?

---

_Comment by @github-actions[bot] on 2023-11-12 18:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+214 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+87 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/callbacks/callback_requests.py#L120'>airflow/callbacks/callback_requests.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/callbacks/callback_requests.py#L80'>airflow/callbacks/callback_requests.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/branch_external_python.py#L36'>airflow/decorators/branch_external_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/branch_python.py#L36'>airflow/decorators/branch_python.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/branch_virtualenv.py#L36'>airflow/decorators/branch_virtualenv.py:36:46:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/external_python.py#L38'>airflow/decorators/external_python.py:38:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/python.py#L63'>airflow/decorators/python.py:63:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/python_virtualenv.py#L37'>airflow/decorators/python_virtualenv.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/decorators/short_circuit.py#L37'>airflow/decorators/short_circuit.py:37:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/lineage/entities.py#L73'>airflow/lineage/entities.py:73:21:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/baseoperator.py#L1489'>airflow/models/baseoperator.py:1489:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dag.py#L2875'>airflow/models/dag.py:2875:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dag.py#L3842'>airflow/models/dag.py:3842:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dag.py#L460'>airflow/models/dag.py:460:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dagbag.py#L101'>airflow/models/dagbag.py:101:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dagbag.py#L98'>airflow/models/dagbag.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dagbag.py#L99'>airflow/models/dagbag.py:99:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dagrun.py#L213'>airflow/models/dagrun.py:213:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/dagrun.py#L380'>airflow/models/dagrun.py:380:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/a9a1b0d990931865507c252ac31daf263c0ce054/airflow/models/taskinstance.py#L2606'>airflow/models/taskinstance.py:2606:9:</a> FBT001 Boolean-typed positional argument in function definition
... 67 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/embed/util.py#L183'>src/bokeh/embed/util.py:183:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/tornado.py#L608'>src/bokeh/server/tornado.py:608:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/settings.py#L172'>src/bokeh/settings.py:172:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:41:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/test_settings.py#L131'>tests/unit/bokeh/test_settings.py:131:33:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/test_settings.py#L135'>tests/unit/bokeh/test_settings.py:135:39:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/analytics/views/support.py#L172'>analytics/views/support.py:172:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/corporate/tests/test_stripe.py#L443'>corporate/tests/test_stripe.py:443:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/actions/create_user.py#L414'>zerver/actions/create_user.py:414:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/actions/user_settings.py#L400'>zerver/actions/user_settings.py:400:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/actions/user_status.py#L11'>zerver/actions/user_status.py:11:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/create_user.py#L159'>zerver/lib/create_user.py:159:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/create_user.py#L164'>zerver/lib/create_user.py:164:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/event_schema.py#L1561'>zerver/lib/event_schema.py:1561:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/export.py#L1931'>zerver/lib/export.py:1931:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/export.py#L2392'>zerver/lib/export.py:2392:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/management.py#L120'>zerver/lib/management.py:120:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/message.py#L1743'>zerver/lib/message.py:1743:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/message.py#L1768'>zerver/lib/message.py:1768:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/message.py#L1790'>zerver/lib/message.py:1790:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/notification_data.py#L266'>zerver/lib/notification_data.py:266:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/streams.py#L94'>zerver/lib/streams.py:94:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/test_classes.py#L1356'>zerver/lib/test_classes.py:1356:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/test_classes.py#L825'>zerver/lib/test_classes.py:825:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/test_runner.py#L188'>zerver/lib/test_runner.py:188:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/lib/webhooks/git.py#L98'>zerver/lib/webhooks/git.py:98:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/tests/test_realm_export.py#L124'>zerver/tests/test_realm_export.py:124:13:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/views/events_register.py#L21'>zerver/views/events_register.py:21:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/views/events_register.py#L46'>zerver/views/events_register.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/views/events_register.py#L50'>zerver/views/events_register.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/views/realm.py#L101'>zerver/views/realm.py:101:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/060b94b71fe5eb0f600420b09fc0480b173cbae4/zerver/views/realm.py#L112'>zerver/views/realm.py:112:5:</a> FBT001 Boolean-typed positional argument in function definition
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 214 | 214 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2023-11-12 19:15_

---

_Merged by @charliermarsh on 2023-11-12 20:09_

---

_Closed by @charliermarsh on 2023-11-12 20:09_

---

_Label `rule` added by @charliermarsh on 2023-11-12 20:09_

---

_Branch deleted on 2023-11-12 20:09_

---

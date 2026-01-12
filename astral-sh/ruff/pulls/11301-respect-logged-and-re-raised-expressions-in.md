```yaml
number: 11301
title: Respect logged and re-raised expressions in nested statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ble
created_at: 2024-05-06T01:37:47Z
updated_at: 2024-05-06T01:52:10Z
url: https://github.com/astral-sh/ruff/pull/11301
synced_at: 2026-01-12T15:55:37Z
```

# Respect logged and re-raised expressions in nested statements

---

_@charliermarsh_

## Summary

Historically, we only ignored `flake8-blind-except` if you re-raised or logged the exception as a _direct_ child statement; but it could be nested somewhere. This was just a known limitation at the time of adding the previous logic.

Closes https://github.com/astral-sh/ruff/issues/11289.


---

_Label `bug` added by @charliermarsh on 2024-05-06 01:37_

---

_Comment by @github-actions[bot] on 2024-05-06 01:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -15 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/cli/cli_parser.py#L78'>airflow/cli/cli_parser.py:78:8:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/cli/commands/task_command.py#L691'>airflow/cli/commands/task_command.py:691:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/amazon/aws/log/s3_task_handler.py#L193'>airflow/providers/amazon/aws/log/s3_task_handler.py:193:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/google/cloud/hooks/datafusion.py#L578'>airflow/providers/google/cloud/hooks/datafusion.py:578:24:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/weaviate/hooks/weaviate.py#L240'>airflow/providers/weaviate/hooks/weaviate.py:240:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/sensors/base.py#L289'>airflow/sensors/base.py:289:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/utils/log/file_task_handler.py#L576'>airflow/utils/log/file_task_handler.py:576:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/tests/cli/commands/_common_cli_classes.py#L102'>tests/cli/commands/_common_cli_classes.py:102:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/tests/sensors/test_external_task_sensor.py#L531'>tests/sensors/test_external_task_sensor.py:531:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/function.py#L141'>src/bokeh/application/handlers/function.py:141:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L110'>src/bokeh/command/bootstrap.py:110:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L67'>tests/support/util/filesystem.py:67:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/corporate/lib/stripe.py#L1199'>corporate/lib/stripe.py:1199:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/zerver/actions/scheduled_messages.py#L385'>zerver/actions/scheduled_messages.py:385:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/zerver/tornado/handlers.py#L80'>zerver/tornado/handlers.py:80:12:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 15 | 0 | 15 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -15 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/cli/cli_parser.py#L78'>airflow/cli/cli_parser.py:78:8:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/cli/commands/task_command.py#L691'>airflow/cli/commands/task_command.py:691:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/amazon/aws/log/s3_task_handler.py#L193'>airflow/providers/amazon/aws/log/s3_task_handler.py:193:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/google/cloud/hooks/datafusion.py#L578'>airflow/providers/google/cloud/hooks/datafusion.py:578:24:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/providers/weaviate/hooks/weaviate.py#L240'>airflow/providers/weaviate/hooks/weaviate.py:240:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/sensors/base.py#L289'>airflow/sensors/base.py:289:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/airflow/utils/log/file_task_handler.py#L576'>airflow/utils/log/file_task_handler.py:576:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/tests/cli/commands/_common_cli_classes.py#L102'>tests/cli/commands/_common_cli_classes.py:102:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/4a568d308a3b07883d3238ea3548dafc9192d8b4/tests/sensors/test_external_task_sensor.py#L531'>tests/sensors/test_external_task_sensor.py:531:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/function.py#L141'>src/bokeh/application/handlers/function.py:141:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L110'>src/bokeh/command/bootstrap.py:110:12:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L67'>tests/support/util/filesystem.py:67:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/corporate/lib/stripe.py#L1199'>corporate/lib/stripe.py:1199:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/zerver/actions/scheduled_messages.py#L385'>zerver/actions/scheduled_messages.py:385:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/zulip/zulip/blob/4efcc33dc24b267383cace92f200b0943f949efc/zerver/tornado/handlers.py#L80'>zerver/tornado/handlers.py:80:12:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 15 | 0 | 15 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-05-06 01:52_

---

_Closed by @charliermarsh on 2024-05-06 01:52_

---

_Branch deleted on 2024-05-06 01:52_

---

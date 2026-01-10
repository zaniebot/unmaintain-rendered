```yaml
number: 9594
title: "[`pylint`] Add fix for `collapsible-else-if` (`PLR5501`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-fix-for-PLR5501
created_at: 2024-01-20T22:44:00Z
updated_at: 2024-01-22T00:53:15Z
url: https://github.com/astral-sh/ruff/pull/9594
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Add fix for `collapsible-else-if` (`PLR5501`)

---

_Pull request opened by @diceroll123 on 2024-01-20 22:44_

## Summary

adds a fix for `collapsible-else-if` / `PLR5501`

## Test Plan

`cargo test`

---

_Renamed from "[pylint] - add fix for `collapsible-else-if` / `PLR5501`" to "[`pylint`] - add fix for `collapsible-else-if` / `PLR5501`" by @diceroll123 on 2024-01-20 22:56_

---

_Comment by @github-actions[bot] on 2024-01-20 23:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +142 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +98 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/configuration.py#L628'>airflow/configuration.py:628:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/configuration.py#L628'>airflow/configuration.py:628:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/jobs/backfill_job_runner.py#L521'>airflow/jobs/backfill_job_runner.py:521:17:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/jobs/backfill_job_runner.py#L521'>airflow/jobs/backfill_job_runner.py:521:17:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dag.py#L1850'>airflow/models/dag.py:1850:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dag.py#L1850'>airflow/models/dag.py:1850:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dag.py#L2495'>airflow/models/dag.py:2495:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dag.py#L2495'>airflow/models/dag.py:2495:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dagrun.py#L486'>airflow/models/dagrun.py:486:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/dagrun.py#L486'>airflow/models/dagrun.py:486:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/variable.py#L144'>airflow/models/variable.py:144:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/models/variable.py#L144'>airflow/models/variable.py:144:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/operators/bash.py#L201'>airflow/operators/bash.py:201:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/operators/bash.py#L201'>airflow/operators/bash.py:201:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/alibaba/cloud/sensors/oss_key.py#L83'>airflow/providers/alibaba/cloud/sensors/oss_key.py:83:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/alibaba/cloud/sensors/oss_key.py#L83'>airflow/providers/alibaba/cloud/sensors/oss_key.py:83:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/hooks/sagemaker.py#L1154'>airflow/providers/amazon/aws/hooks/sagemaker.py:1154:17:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/hooks/sagemaker.py#L1154'>airflow/providers/amazon/aws/hooks/sagemaker.py:1154:17:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/operators/batch.py#L381'>airflow/providers/amazon/aws/operators/batch.py:381:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/operators/batch.py#L381'>airflow/providers/amazon/aws/operators/batch.py:381:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/operators/eks.py#L1050'>airflow/providers/amazon/aws/operators/eks.py:1050:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/operators/eks.py#L1050'>airflow/providers/amazon/aws/operators/eks.py:1050:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/sensors/s3.py#L153'>airflow/providers/amazon/aws/sensors/s3.py:153:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/sensors/s3.py#L153'>airflow/providers/amazon/aws/sensors/s3.py:153:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/sensors/s3.py#L349'>airflow/providers/amazon/aws/sensors/s3.py:349:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/sensors/s3.py#L349'>airflow/providers/amazon/aws/sensors/s3.py:349:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/transfers/ftp_to_s3.py#L143'>airflow/providers/amazon/aws/transfers/ftp_to_s3.py:143:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/amazon/aws/transfers/ftp_to_s3.py#L143'>airflow/providers/amazon/aws/transfers/ftp_to_s3.py:143:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L533'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:533:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L533'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:533:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/elasticsearch/log/es_task_handler.py#L449'>airflow/providers/elasticsearch/log/es_task_handler.py:449:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/elasticsearch/log/es_task_handler.py#L449'>airflow/providers/elasticsearch/log/es_task_handler.py:449:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/google/cloud/operators/datacatalog.py#L385'>airflow/providers/google/cloud/operators/datacatalog.py:385:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/google/cloud/operators/datacatalog.py#L385'>airflow/providers/google/cloud/operators/datacatalog.py:385:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/b5b5852cceebac8d66849e6afdfa33e7cb57e7dd/tests/integration/logs/test_logs_command.py#L218'>tests/integration/logs/test_logs_command.py:218:17:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5b5852cceebac8d66849e6afdfa33e7cb57e7dd/tests/integration/logs/test_logs_command.py#L218'>tests/integration/logs/test_logs_command.py:218:17:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/session.py#L454'>src/bokeh/client/session.py:454:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/session.py#L454'>src/bokeh/client/session.py:454:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/query.py#L210'>src/bokeh/core/query.py:210:17:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/query.py#L210'>src/bokeh/core/query.py:210:17:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L718'>src/bokeh/core/serialization.py:718:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L718'>src/bokeh/core/serialization.py:718:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:5:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:5:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/server/tornado.py#L315'>src/bokeh/server/tornado.py:315:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/server/tornado.py#L315'>src/bokeh/server/tornado.py:315:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +26 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/analytics/views/stats.py#L280'>analytics/views/stats.py:280:5:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/analytics/views/stats.py#L280'>analytics/views/stats.py:280:5:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4553'>corporate/lib/stripe.py:4553:9:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4553'>corporate/lib/stripe.py:4553:9:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/scripts/lib/zulip_tools.py#L202'>scripts/lib/zulip_tools.py:202:5:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/scripts/lib/zulip_tools.py#L202'>scripts/lib/zulip_tools.py:202:5:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/lib/template_parser.py#L512'>tools/lib/template_parser.py:512:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/lib/template_parser.py#L512'>tools/lib/template_parser.py:512:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/actions/message_edit.py#L1070'>zerver/actions/message_edit.py:1070:13:</a> PLR5501 Use `elif` instead of `else` then `if`, to reduce indentation
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/actions/message_edit.py#L1070'>zerver/actions/message_edit.py:1070:13:</a> PLR5501 [*] Use `elif` instead of `else` then `if`, to reduce indentation
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR5501 | 142 | 0 | 0 | 142 | 0 |

</p>
</details>




---

_Renamed from "[`pylint`] - add fix for `collapsible-else-if` / `PLR5501`" to "[`pylint`] - add fix for `collapsible-else-if` (`PLR5501`)" by @diceroll123 on 2024-01-21 04:33_

---

_Renamed from "[`pylint`] - add fix for `collapsible-else-if` (`PLR5501`)" to "[`pylint`] - Add fix for `collapsible-else-if` (`PLR5501`)" by @diceroll123 on 2024-01-21 18:44_

---

_@charliermarsh approved on 2024-01-22 00:39_

---

_Label `autofix` added by @charliermarsh on 2024-01-22 00:40_

---

_Label `preview` added by @charliermarsh on 2024-01-22 00:40_

---

_Renamed from "[`pylint`] - Add fix for `collapsible-else-if` (`PLR5501`)" to "[`pylint`] Add fix for `collapsible-else-if` (`PLR5501`)" by @charliermarsh on 2024-01-22 00:40_

---

_Merged by @charliermarsh on 2024-01-22 00:53_

---

_Closed by @charliermarsh on 2024-01-22 00:53_

---

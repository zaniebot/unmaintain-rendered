```yaml
number: 9844
title: "[`pylint`] - Implement `too-many-attributes rule` (`PLR0902`)"
type: pull_request
state: closed
author: liushuyu
labels:
  - rule
assignees: []
base: main
head: main
created_at: 2024-02-05T20:33:04Z
updated_at: 2024-03-28T16:05:37Z
url: https://github.com/astral-sh/ruff/pull/9844
synced_at: 2026-01-12T15:55:30Z
```

# [`pylint`] - Implement `too-many-attributes rule` (`PLR0902`)

---

_@liushuyu_

## Summary

Adds too-many-attributes rule (PLR0902) from Pylint. This implementation has a similar limitation as the Pylint one, where dynamically generated class attributes (e.g. through `setattr` or `eval`) are not considered.

## Test Plan

A new test has been added and can be executed using `cargo test`.


---

_Comment by @github-actions[bot] on 2024-02-05 20:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+806 -0 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+585 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/cli/commands/webserver_command.py#L48'>airflow/cli/commands/webserver_command.py:48:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/dag_processing/manager.py#L330'>airflow/dag_processing/manager.py:330:7:</a> PLR0902 Too many instance attributes (27/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/dag_processing/manager.py#L99'>airflow/dag_processing/manager.py:99:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/dag_processing/processor.py#L69'>airflow/dag_processing/processor.py:69:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/jobs/backfill_job_runner.py#L65'>airflow/jobs/backfill_job_runner.py:65:7:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/jobs/job.py#L58'>airflow/jobs/job.py:58:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/jobs/local_task_job_runner.py#L77'>airflow/jobs/local_task_job_runner.py:77:7:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/jobs/scheduler_job_runner.py#L129'>airflow/jobs/scheduler_job_runner.py:129:7:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/baseoperator.py#L478'>airflow/models/baseoperator.py:478:7:</a> PLR0902 Too many instance attributes (53/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/connection.py#L102'>airflow/models/connection.py:102:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/dag.py#L302'>airflow/models/dag.py:302:7:</a> PLR0902 Too many instance attributes (42/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/dagbag.py#L79'>airflow/models/dagbag.py:79:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/dagrun.py#L110'>airflow/models/dagrun.py:110:7:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/taskinstance.py#L1212'>airflow/models/taskinstance.py:1212:7:</a> PLR0902 Too many instance attributes (19/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/taskinstance.py#L3520'>airflow/models/taskinstance.py:3520:7:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/models/taskreschedule.py#L44'>airflow/models/taskreschedule.py:44:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/operators/email.py#L29'>airflow/operators/email.py:29:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/operators/trigger_dagrun.py#L72'>airflow/operators/trigger_dagrun.py:72:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/airbyte/operators/airbyte.py#L33'>airflow/providers/airbyte/operators/airbyte.py:33:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L68'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:68:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/hooks/glue.py#L33'>airflow/providers/amazon/aws/hooks/glue.py:33:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/appflow.py#L45'>airflow/providers/amazon/aws/operators/appflow.py:45:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/athena.py#L40'>airflow/providers/amazon/aws/operators/athena.py:40:7:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/batch.py#L428'>airflow/providers/amazon/aws/operators/batch.py:428:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/batch.py#L54'>airflow/providers/amazon/aws/operators/batch.py:54:7:</a> PLR0902 Too many instance attributes (22/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/datasync.py#L35'>airflow/providers/amazon/aws/operators/datasync.py:35:7:</a> PLR0902 Too many instance attributes (20/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/dms.py#L30'>airflow/providers/amazon/aws/operators/dms.py:30:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/ec2.py#L130'>airflow/providers/amazon/aws/operators/ec2.py:130:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/ecs.py#L355'>airflow/providers/amazon/aws/operators/ecs.py:355:7:</a> PLR0902 Too many instance attributes (26/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L140'>airflow/providers/amazon/aws/operators/eks.py:140:7:</a> PLR0902 Too many instance attributes (18/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L436'>airflow/providers/amazon/aws/operators/eks.py:436:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L561'>airflow/providers/amazon/aws/operators/eks.py:561:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L675'>airflow/providers/amazon/aws/operators/eks.py:675:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L807'>airflow/providers/amazon/aws/operators/eks.py:807:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/eks.py#L898'>airflow/providers/amazon/aws/operators/eks.py:898:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/1b121203cca5ea6f7ea3d3ba3bee63bca1786709/airflow/providers/amazon/aws/operators/emr.py#L1019'>airflow/providers/amazon/aws/operators/emr.py:1019:7:</a> PLR0902 Too many instance attributes (10/7)
... 549 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+87 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/build/build_context.py#L53'>samcli/commands/build/build_context.py:53:7:</a> PLR0902 Too many instance attributes (32/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/delete/delete_context.py#L30'>samcli/commands/delete/delete_context.py:30:7:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/deploy/deploy_context.py#L43'>samcli/commands/deploy/deploy_context.py:43:7:</a> PLR0902 Too many instance attributes (28/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/deploy/guided_context.py#L42'>samcli/commands/deploy/guided_context.py:42:7:</a> PLR0902 Too many instance attributes (32/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/list/endpoints/endpoints_context.py#L16'>samcli/commands/list/endpoints/endpoints_context.py:16:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/local/cli_common/invoke_context.py#L62'>samcli/commands/local/cli_common/invoke_context.py:62:7:</a> PLR0902 Too many instance attributes (35/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/local/lib/local_api_service.py#L15'>samcli/commands/local/lib/local_api_service.py:15:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/local/lib/local_lambda.py#L34'>samcli/commands/local/lib/local_lambda.py:34:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/package/package_context.py#L44'>samcli/commands/package/package_context.py:44:7:</a> PLR0902 Too many instance attributes (18/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/e43a354fd784667e81e8ffaa2439c0b22d47e60e/samcli/commands/pipeline/bootstrap/guided_context.py#L36'>samcli/commands/pipeline/bootstrap/guided_context.py:36:7:</a> PLR0902 Too many instance attributes (16/7)
... 77 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L27'>release/config.py:27:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L53'>src/bokeh/application/handlers/code_runner.py:53:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L76'>src/bokeh/client/connection.py:76:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L86'>src/bokeh/document/callbacks.py:86:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L111'>src/bokeh/document/document.py:111:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L96'>src/bokeh/models/util/structure.py:96:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L118'>src/bokeh/protocol/message.py:118:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L264'>src/bokeh/resources.py:264:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L144'>src/bokeh/server/contexts.py:144:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L122'>src/bokeh/server/session.py:122:7:</a> PLR0902 Too many instance attributes (13/7)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/journalist_gui/journalist_gui/SecureDropUpdater.py#L169'>journalist_gui/journalist_gui/SecureDropUpdater.py:169:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/journalist_gui/journalist_gui/updaterUI.py#L10'>journalist_gui/journalist_gui/updaterUI.py:10:7:</a> PLR0902 Too many instance attributes (16/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/journalist_app/sessions.py#L18'>securedrop/journalist_app/sessions.py:18:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/journalist_app/sessions.py#L83'>securedrop/journalist_app/sessions.py:83:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/pretty_bad_protocol/_meta.py#L110'>securedrop/pretty_bad_protocol/_meta.py:110:7:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/pretty_bad_protocol/_parsers.py#L1172'>securedrop/pretty_bad_protocol/_parsers.py:1172:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/pretty_bad_protocol/_parsers.py#L1457'>securedrop/pretty_bad_protocol/_parsers.py:1457:7:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/pretty_bad_protocol/_parsers.py#L978'>securedrop/pretty_bad_protocol/_parsers.py:978:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/secure_tempfile.py#L13'>securedrop/secure_tempfile.py:13:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/securedrop/upload-screenshots.py#L72'>securedrop/upload-screenshots.py:72:7:</a> PLR0902 Too many instance attributes (9/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/auth.py#L22'>blinkpy/auth.py:22:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/blinkpy.py#L41'>blinkpy/blinkpy.py:41:7:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/camera.py#L19'>blinkpy/camera.py:19:7:</a> PLR0902 Too many instance attributes (23/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/sync_module.py#L23'>blinkpy/sync_module.py:23:7:</a> PLR0902 Too many instance attributes (21/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/tests/mock_responses.py#L5'>tests/mock_responses.py:5:7:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/2cb5caf61ca0a5ac4a6d920f55908c3c42a846a2/ibis/backends/bigquery/udf/core.py#L105'>ibis/backends/bigquery/udf/core.py:105:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/ibis-project/ibis/blob/2cb5caf61ca0a5ac4a6d920f55908c3c42a846a2/ibis/backends/flink/ddl.py#L77'>ibis/backends/flink/ddl.py:77:7:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/ibis-project/ibis/blob/2cb5caf61ca0a5ac4a6d920f55908c3c42a846a2/ibis/backends/impala/ddl.py#L73'>ibis/backends/impala/ddl.py:73:7:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/bulk_writer.py#L36'>pymilvus/bulk_writer/bulk_writer.py:36:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/remote_bulk_writer.py#L38'>pymilvus/bulk_writer/remote_bulk_writer.py:38:11:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/client/abstract.py#L14'>pymilvus/client/abstract.py:14:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/client/abstract.py#L185'>pymilvus/client/abstract.py:185:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/client/abstract.py#L99'>pymilvus/client/abstract.py:99:7:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/client/asynch.py#L56'>pymilvus/client/asynch.py:56:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/client/grpc_handler.py#L68'>pymilvus/client/grpc_handler.py:68:7:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/orm/iterator.py#L280'>pymilvus/orm/iterator.py:280:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/orm/iterator.py#L59'>pymilvus/orm/iterator.py:59:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/orm/schema.py#L255'>pymilvus/orm/schema.py:255:7:</a> PLR0902 Too many instance attributes (10/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+50 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/asv_bench/benchmarks/categoricals.py#L18'>asv_bench/benchmarks/categoricals.py:18:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/asv_bench/benchmarks/io/hdf.py#L14'>asv_bench/benchmarks/io/hdf.py:14:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/asv_bench/benchmarks/join_merge.py#L367'>asv_bench/benchmarks/join_merge.py:367:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/asv_bench/benchmarks/join_merge.py#L427'>asv_bench/benchmarks/join_merge.py:427:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/asv_bench/benchmarks/reindex.py#L13'>asv_bench/benchmarks/reindex.py:13:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/core/apply.py#L115'>pandas/core/apply.py:115:7:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/core/groupby/groupby.py#L1040'>pandas/core/groupby/groupby.py:1040:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/core/groupby/grouper.py#L453'>pandas/core/groupby/grouper.py:453:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/core/groupby/grouper.py#L58'>pandas/core/groupby/grouper.py:58:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/core/resample.py#L122'>pandas/core/resample.py:122:7:</a> PLR0902 Too many instance attributes (13/7)
... 40 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0902 | 806 | 806 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:71 on 2024-02-26 12:56_

We should probably also collect attributes that are defined on the class itself


```python
class Test:
	a: str 
	b
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:58 on 2024-02-26 12:58_

We should probably handle `AnnAssign` too:

```python
        self.a: int = 190
```

---

_@MichaReiser reviewed on 2024-02-26 13:04_

Thanks for contributing this rule.

Code: There are two cases where the rule currently misses field declarations that should be supported. 

An open question for us as maintainers is whether we want to merge the rule today. The problem we're facing is that many users use `--select ALL` and consider it as Ruff's "recommended" rule set. I see that this rule is useful when used in a mindful way, but it could guide less aware users in the direction that having classes with many attributes is always bad and is generally considered bad practice. 

We hope to solve the `ALL` problem by recategorizing our rules so that more opinionated rules require explicit opt-in.  But we may need to wait to add new rules till then.

---

_@liushuyu reviewed on 2024-02-26 18:54_

---

_Review comment by @liushuyu on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:71 on 2024-02-26 18:54_

Addressed. Test case added.

---

_Review comment by @liushuyu on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:58 on 2024-02-26 18:55_

Addressed. Test case added.

---

_@liushuyu reviewed on 2024-02-26 18:55_

---

_Comment by @liushuyu on 2024-02-26 18:56_

> We hope to solve the `ALL` problem by recategorizing our rules so that more opinionated rules require explicit opt-in. But we may need to wait to add new rules till then.

This lint is current in the experimental state, so it won't be enabled by default ...?
Although I agree enabling this lint by default without thinking is a bad idea (but could be helpful for those who are migrating from Pylint to reach 1:1 compatibility).

---

_Review requested from @MichaReiser by @liushuyu on 2024-02-26 19:08_

---

_Comment by @MichaReiser on 2024-02-26 19:21_

> This lint is current in the experimental state, so it won't be enabled by default ...?

That's correct. But the intent would be to stabilize the rule eventually (ideally one or two minor releases later). 

---

_Comment by @liushuyu on 2024-02-26 19:33_

> > This lint is current in the experimental state, so it won't be enabled by default ...?
> 
> That's correct. But the intent would be to stabilize the rule eventually (ideally one or two minor releases later).

Understood. So this pull request would be put on the backburner and wait until a better grouping mechanism is added?

---

_Comment by @MichaReiser on 2024-02-27 08:23_

I discussed this internally and @charliermarsh is fine with adding new pylint rules, even if they are very opinionated.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/too_many_attributes.py`:5 on 2024-02-27 08:24_

Nice catch. I wasn't aware that this is allowed too

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:316 on 2024-02-27 08:24_

Should the rule name be `too-many-instance-attributes` to match [pylint's](https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/too-many-instance-attributes.html) name?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:66 on 2024-02-27 08:32_

I suspect that we don't need the `scope` here, because it's given that we're inside of a class, because we get the `class_def`

---

_@MichaReiser approved on 2024-02-27 08:33_

This looks good to me but I want that @charliermarsh takes a look because I wonder if we extract the variables already as part of the semantic model ( I tried `scope.bindings` but this doesn't give me all fields). 

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-27 08:33_

---

_Label `rule` added by @MichaReiser on 2024-02-27 08:33_

---

_@MichaReiser reviewed on 2024-02-27 08:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/too_many_attributes.py`:24 on 2024-02-27 08:35_

Can we add a few more tests:

* a test for a class that has exactly 7 attributes
* a test for a class that assigns to attributes in method other than `__init__`
* a test for a class that has fewer than 7 attributes
* a test for a class that assigns the same variable multiple times (e.g. defines the attributes and assigns in `__init__`

---

_Review requested from @MichaReiser by @liushuyu on 2024-03-05 18:40_

---

_@liushuyu reviewed on 2024-03-05 18:40_

---

_Review comment by @liushuyu on `crates/ruff_linter/src/rules/pylint/rules/too_many_attributes.rs`:66 on 2024-03-05 18:40_

Addressed.

---

_@liushuyu reviewed on 2024-03-05 18:41_

---

_Review comment by @liushuyu on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:316 on 2024-03-05 18:41_

Done.

---

_Comment by @liushuyu on 2024-03-05 18:44_

Comments addressed and rebased against the latest main branch.

---

_Comment by @MichaReiser on 2024-03-20 09:56_

I think this is the same as https://github.com/astral-sh/ruff/pull/10431. We need to decide which of the two PRs we want to move forward.

---

_Comment by @diceroll123 on 2024-03-23 03:41_

> I think this is the same as #10431. We need to decide which of the two PRs we want to move forward.

I hadn't noticed this PR existed before making my own, I concede to this one!

---

_Comment by @MichaReiser on 2024-03-28 16:05_

Thank you for contributing to this new rule, and I am sorry that it took so long to decide.

 We agree that the rule itself is useful, and we're open to merging the rule once #1774 is complete, but we don't want to merge the rule today. The reasons are:

* The rule is very opinionated. It intentionally restricts Python functionality by disallowing classes with more than X attributes. This is fine, but we currently have no good model to group such opinionated rules, and we don't want users using `ALL` or selecting pylint to enable this rule by default because we believe that would be misleading and not all users have the required knowledge to know that this rule might just not be for them. 
* The rule measures complexity similar to the mc-cabe rule. It is different in that it measures the complexity of a structure and not a control flow complexity. However, we must figure out how we want different metrics to measure complexity. Should these be separate rules or a single, configurable rule? 

I'm sorry that it took us so long to come to that conclusion and that we haven't flagged this on the pylint issue. I plan to go through the Pylint rules and mark the rules that are unlikely to be accepted today, but I first have to catch up on open linter PRs. 

Thank you again for contributing and I hope we can merge this rule in the future, once we figured out the right place for it.



---

_Closed by @MichaReiser on 2024-03-28 16:05_

---

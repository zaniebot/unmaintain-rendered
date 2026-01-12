```yaml
number: 10148
title: "[`ruff`] Expand rule for `list(iterable).pop(0)` idiom (`RUF015`)"
type: pull_request
state: merged
author: robincaloudis
labels:
  - rule
assignees: []
merged: true
base: main
head: rc-extension-ruf015
created_at: 2024-02-27T22:21:25Z
updated_at: 2024-02-28T00:29:31Z
url: https://github.com/astral-sh/ruff/pull/10148
synced_at: 2026-01-12T15:55:31Z
```

# [`ruff`] Expand rule for `list(iterable).pop(0)` idiom (`RUF015`)

---

_@robincaloudis_

## Summary

Currently, rule `RUF015` is not able to detect the usage of `list(iterable).pop(0)` falling under the category of an _unnecessary iterable allocation for accessing the first element_. This PR wants to change that. See the underlying issue for more details.

* Provide extension to detect `list(iterable).pop(0)`, but not `list(iterable).pop(i)` where i > 1
* Update corresponding doc

## Test Plan

* `RUF015.py` and the corresponding snap file were extended such that their correspond to the new behaviour

Closes https://github.com/astral-sh/ruff/issues/9190

--- 

PS: I've only been working on this ticket as I haven't seen any activity from issue assignee @rmad17, neither in this repo nor in a fork. I hope I interpreted his inactivity correctly. Didn't mean to steal his chance. Since I stumbled across the underlying problem myself, I wanted to offer a solution as soon as possible.

---

_Comment by @github-actions[bot] on 2024-02-27 22:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -802 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -585 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/cli/commands/webserver_command.py#L48'>airflow/cli/commands/webserver_command.py:48:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/dag_processing/manager.py#L330'>airflow/dag_processing/manager.py:330:7:</a> PLR0902 Too many instance attributes (27/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/dag_processing/manager.py#L99'>airflow/dag_processing/manager.py:99:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/dag_processing/processor.py#L69'>airflow/dag_processing/processor.py:69:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/jobs/backfill_job_runner.py#L65'>airflow/jobs/backfill_job_runner.py:65:7:</a> PLR0902 Too many instance attributes (17/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/jobs/job.py#L58'>airflow/jobs/job.py:58:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/jobs/local_task_job_runner.py#L77'>airflow/jobs/local_task_job_runner.py:77:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/jobs/scheduler_job_runner.py#L129'>airflow/jobs/scheduler_job_runner.py:129:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/baseoperator.py#L477'>airflow/models/baseoperator.py:477:7:</a> PLR0902 Too many instance attributes (53/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/connection.py#L97'>airflow/models/connection.py:97:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/dag.py#L302'>airflow/models/dag.py:302:7:</a> PLR0902 Too many instance attributes (42/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/dagbag.py#L79'>airflow/models/dagbag.py:79:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/dagrun.py#L110'>airflow/models/dagrun.py:110:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/taskinstance.py#L1211'>airflow/models/taskinstance.py:1211:7:</a> PLR0902 Too many instance attributes (20/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/taskinstance.py#L3518'>airflow/models/taskinstance.py:3518:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/models/taskreschedule.py#L44'>airflow/models/taskreschedule.py:44:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/operators/email.py#L29'>airflow/operators/email.py:29:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/operators/trigger_dagrun.py#L72'>airflow/operators/trigger_dagrun.py:72:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/airbyte/operators/airbyte.py#L33'>airflow/providers/airbyte/operators/airbyte.py:33:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L68'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:68:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/hooks/glue.py#L33'>airflow/providers/amazon/aws/hooks/glue.py:33:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/appflow.py#L45'>airflow/providers/amazon/aws/operators/appflow.py:45:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/athena.py#L40'>airflow/providers/amazon/aws/operators/athena.py:40:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/batch.py#L428'>airflow/providers/amazon/aws/operators/batch.py:428:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/batch.py#L54'>airflow/providers/amazon/aws/operators/batch.py:54:7:</a> PLR0902 Too many instance attributes (22/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/datasync.py#L35'>airflow/providers/amazon/aws/operators/datasync.py:35:7:</a> PLR0902 Too many instance attributes (20/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/dms.py#L30'>airflow/providers/amazon/aws/operators/dms.py:30:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/ec2.py#L122'>airflow/providers/amazon/aws/operators/ec2.py:122:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/ecs.py#L355'>airflow/providers/amazon/aws/operators/ecs.py:355:7:</a> PLR0902 Too many instance attributes (26/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L140'>airflow/providers/amazon/aws/operators/eks.py:140:7:</a> PLR0902 Too many instance attributes (18/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L436'>airflow/providers/amazon/aws/operators/eks.py:436:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L561'>airflow/providers/amazon/aws/operators/eks.py:561:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L675'>airflow/providers/amazon/aws/operators/eks.py:675:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L807'>airflow/providers/amazon/aws/operators/eks.py:807:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/eks.py#L898'>airflow/providers/amazon/aws/operators/eks.py:898:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/051720401d37d47c9c86867fd3d044b899b8b0ca/airflow/providers/amazon/aws/operators/emr.py#L1001'>airflow/providers/amazon/aws/operators/emr.py:1001:7:</a> PLR0902 Too many instance attributes (10/7)
... 549 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -84 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/delete/delete_context.py#L30'>samcli/commands/delete/delete_context.py:30:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/deploy/deploy_context.py#L43'>samcli/commands/deploy/deploy_context.py:43:7:</a> PLR0902 Too many instance attributes (28/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/deploy/guided_context.py#L42'>samcli/commands/deploy/guided_context.py:42:7:</a> PLR0902 Too many instance attributes (32/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/list/endpoints/endpoints_context.py#L16'>samcli/commands/list/endpoints/endpoints_context.py:16:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/local/cli_common/invoke_context.py#L62'>samcli/commands/local/cli_common/invoke_context.py:62:7:</a> PLR0902 Too many instance attributes (35/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/local/lib/local_api_service.py#L15'>samcli/commands/local/lib/local_api_service.py:15:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/local/lib/local_lambda.py#L34'>samcli/commands/local/lib/local_lambda.py:34:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/package/package_context.py#L44'>samcli/commands/package/package_context.py:44:7:</a> PLR0902 Too many instance attributes (18/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/pipeline/bootstrap/guided_context.py#L36'>samcli/commands/pipeline/bootstrap/guided_context.py:36:7:</a> PLR0902 Too many instance attributes (16/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/iac/plugins_interfaces.py#L182'>samcli/lib/iac/plugins_interfaces.py:182:7:</a> PLR0902 Too many instance attributes (8/7)
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L27'>release/config.py:27:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L53'>src/bokeh/application/handlers/code_runner.py:53:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L76'>src/bokeh/client/connection.py:76:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L86'>src/bokeh/document/callbacks.py:86:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L111'>src/bokeh/document/document.py:111:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L96'>src/bokeh/models/util/structure.py:96:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L118'>src/bokeh/protocol/message.py:118:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L264'>src/bokeh/resources.py:264:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L144'>src/bokeh/server/contexts.py:144:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L122'>src/bokeh/server/session.py:122:7:</a> PLR0902 Too many instance attributes (13/7)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/journalist_gui/journalist_gui/SecureDropUpdater.py#L169'>journalist_gui/journalist_gui/SecureDropUpdater.py:169:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/journalist_gui/journalist_gui/updaterUI.py#L10'>journalist_gui/journalist_gui/updaterUI.py:10:7:</a> PLR0902 Too many instance attributes (16/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/journalist_app/sessions.py#L18'>securedrop/journalist_app/sessions.py:18:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/journalist_app/sessions.py#L83'>securedrop/journalist_app/sessions.py:83:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_meta.py#L110'>securedrop/pretty_bad_protocol/_meta.py:110:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_parsers.py#L1172'>securedrop/pretty_bad_protocol/_parsers.py:1172:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_parsers.py#L1457'>securedrop/pretty_bad_protocol/_parsers.py:1457:7:</a> PLR0902 Too many instance attributes (17/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_parsers.py#L978'>securedrop/pretty_bad_protocol/_parsers.py:978:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/secure_tempfile.py#L13'>securedrop/secure_tempfile.py:13:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/upload-screenshots.py#L72'>securedrop/upload-screenshots.py:72:7:</a> PLR0902 Too many instance attributes (9/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/auth.py#L22'>blinkpy/auth.py:22:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/blinkpy.py#L41'>blinkpy/blinkpy.py:41:7:</a> PLR0902 Too many instance attributes (17/7)
- <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/camera.py#L19'>blinkpy/camera.py:19:7:</a> PLR0902 Too many instance attributes (23/7)
- <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinkpy/sync_module.py#L23'>blinkpy/sync_module.py:23:7:</a> PLR0902 Too many instance attributes (21/7)
- <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/tests/mock_responses.py#L5'>tests/mock_responses.py:5:7:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/bigquery/udf/core.py#L105'>ibis/backends/bigquery/udf/core.py:105:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/flink/ddl.py#L77'>ibis/backends/flink/ddl.py:77:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/impala/ddl.py#L73'>ibis/backends/impala/ddl.py:73:7:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/bulk_writer/bulk_writer.py#L36'>pymilvus/bulk_writer/bulk_writer.py:36:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/bulk_writer/remote_bulk_writer.py#L38'>pymilvus/bulk_writer/remote_bulk_writer.py:38:11:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/abstract.py#L14'>pymilvus/client/abstract.py:14:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/abstract.py#L186'>pymilvus/client/abstract.py:186:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/abstract.py#L99'>pymilvus/client/abstract.py:99:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/asynch.py#L56'>pymilvus/client/asynch.py:56:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/grpc_handler.py#L68'>pymilvus/client/grpc_handler.py:68:7:</a> PLR0902 Too many instance attributes (17/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/orm/iterator.py#L275'>pymilvus/orm/iterator.py:275:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/orm/iterator.py#L58'>pymilvus/orm/iterator.py:58:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/orm/schema.py#L250'>pymilvus/orm/schema.py:250:7:</a> PLR0902 Too many instance attributes (10/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -50 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/asv_bench/benchmarks/categoricals.py#L18'>asv_bench/benchmarks/categoricals.py:18:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/asv_bench/benchmarks/io/hdf.py#L14'>asv_bench/benchmarks/io/hdf.py:14:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/asv_bench/benchmarks/join_merge.py#L367'>asv_bench/benchmarks/join_merge.py:367:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/asv_bench/benchmarks/join_merge.py#L427'>asv_bench/benchmarks/join_merge.py:427:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/asv_bench/benchmarks/reindex.py#L13'>asv_bench/benchmarks/reindex.py:13:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/pandas/core/apply.py#L115'>pandas/core/apply.py:115:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/pandas/core/groupby/groupby.py#L1040'>pandas/core/groupby/groupby.py:1040:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/pandas/core/groupby/grouper.py#L453'>pandas/core/groupby/grouper.py:453:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/pandas/core/groupby/grouper.py#L58'>pandas/core/groupby/grouper.py:58:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/pandas-dev/pandas/blob/015f8d3e2a1d007400a526fa89b373751d244e67/pandas/core/resample.py#L120'>pandas/core/resample.py:120:7:</a> PLR0902 Too many instance attributes (13/7)
... 40 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0902 | 802 | 0 | 802 | 0 | 0 |

</p>
</details>




---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-27 22:48_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-27 22:49_

---

_Label `rule` added by @charliermarsh on 2024-02-27 22:49_

---

_@robincaloudis reviewed on 2024-02-27 22:56_

---

_Review comment by @robincaloudis on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF015_RUF015.py.snap`:3 on 2024-02-27 22:56_

This slipped through and can be removed before merging. I will correct this.

---

_@charliermarsh approved on 2024-02-28 00:15_

Thanks!

---

_Renamed from "[ruff] Expand rule for `list(iterable).pop(0)` idiom (RUF015)" to "[`ruff`] Expand rule for `list(iterable).pop(0)` idiom (`RUF015`)" by @charliermarsh on 2024-02-28 00:16_

---

_Merged by @charliermarsh on 2024-02-28 00:24_

---

_Closed by @charliermarsh on 2024-02-28 00:24_

---

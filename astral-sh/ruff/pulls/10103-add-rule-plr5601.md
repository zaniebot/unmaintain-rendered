```yaml
number: 10103
title: Add rule PLR5601
type: pull_request
state: closed
author: tibor-reiss
labels:
  - rule
assignees: []
base: main
head: add-PLR5601
created_at: 2024-02-23T17:29:35Z
updated_at: 2024-03-28T16:35:59Z
url: https://github.com/astral-sh/ruff/pull/10103
synced_at: 2026-01-12T15:55:31Z
```

# Add rule PLR5601

---

_@tibor-reiss_

Add rule [confusing-consecutive-elif (PLR5601)](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/confusing-consecutive-elif.html)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test plan: `cargo test`

---

_Comment by @github-actions[bot] on 2024-02-23 17:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+161 -802 violations, +0 -0 fixes in 13 projects; 30 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -585 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/cli/commands/db_command.py#L121'>airflow/cli/commands/db_command.py:121:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/cli/commands/db_command.py#L164'>airflow/cli/commands/db_command.py:164:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/cli/commands/task_command.py#L176'>airflow/cli/commands/task_command.py:176:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/cli/commands/webserver_command.py#L48'>airflow/cli/commands/webserver_command.py:48:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/configuration.py#L1536'>airflow/configuration.py:1536:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/dag_processing/manager.py#L330'>airflow/dag_processing/manager.py:330:7:</a> PLR0902 Too many instance attributes (27/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/dag_processing/manager.py#L99'>airflow/dag_processing/manager.py:99:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/dag_processing/processor.py#L69'>airflow/dag_processing/processor.py:69:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/decorators/setup_teardown.py#L72'>airflow/decorators/setup_teardown.py:72:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/jobs/backfill_job_runner.py#L65'>airflow/jobs/backfill_job_runner.py:65:7:</a> PLR0902 Too many instance attributes (17/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/jobs/job.py#L58'>airflow/jobs/job.py:58:7:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/jobs/local_task_job_runner.py#L284'>airflow/jobs/local_task_job_runner.py:284:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/jobs/local_task_job_runner.py#L77'>airflow/jobs/local_task_job_runner.py:77:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/jobs/scheduler_job_runner.py#L129'>airflow/jobs/scheduler_job_runner.py:129:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/baseoperator.py#L477'>airflow/models/baseoperator.py:477:7:</a> PLR0902 Too many instance attributes (53/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/connection.py#L97'>airflow/models/connection.py:97:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/dag.py#L302'>airflow/models/dag.py:302:7:</a> PLR0902 Too many instance attributes (42/7)
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/dag.py#L3476'>airflow/models/dag.py:3476:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/dagbag.py#L79'>airflow/models/dagbag.py:79:7:</a> PLR0902 Too many instance attributes (12/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/dagrun.py#L110'>airflow/models/dagrun.py:110:7:</a> PLR0902 Too many instance attributes (13/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/taskinstance.py#L1211'>airflow/models/taskinstance.py:1211:7:</a> PLR0902 Too many instance attributes (20/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/taskinstance.py#L3518'>airflow/models/taskinstance.py:3518:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/models/taskreschedule.py#L44'>airflow/models/taskreschedule.py:44:7:</a> PLR0902 Too many instance attributes (9/7)
... 570 additional changes omitted for rule PLR0902
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/amazon/aws/auth_manager/aws_auth_manager.py#L480'>airflow/providers/amazon/aws/auth_manager/aws_auth_manager.py:480:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/amazon/aws/operators/eks.py#L115'>airflow/providers/amazon/aws/operators/eks.py:115:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/amazon/aws/sensors/s3.py#L185'>airflow/providers/amazon/aws/sensors/s3.py:185:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/amazon/aws/transfers/gcs_to_s3.py#L158'>airflow/providers/amazon/aws/transfers/gcs_to_s3.py:158:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/apache/spark/hooks/spark_submit.py#L565'>airflow/providers/apache/spark/hooks/spark_submit.py:565:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/fab/auth_manager/security_manager/override.py#L2241'>airflow/providers/fab/auth_manager/security_manager/override.py:2241:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/google/cloud/operators/dataproc.py#L500'>airflow/providers/google/cloud/operators/dataproc.py:500:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/apache/airflow/blob/9e4bdc9e457c275eb2cead5d80c2f79c3b9a0085/airflow/providers/openlineage/utils/utils.py#L91'>airflow/providers/openlineage/utils/utils.py:91:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
... 582 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+6 -84 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/delete/delete_context.py#L137'>samcli/commands/delete/delete_context.py:137:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/delete/delete_context.py#L30'>samcli/commands/delete/delete_context.py:30:7:</a> PLR0902 Too many instance attributes (14/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/deploy/deploy_context.py#L43'>samcli/commands/deploy/deploy_context.py:43:7:</a> PLR0902 Too many instance attributes (28/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/deploy/guided_context.py#L42'>samcli/commands/deploy/guided_context.py:42:7:</a> PLR0902 Too many instance attributes (32/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/list/endpoints/endpoints_context.py#L16'>samcli/commands/list/endpoints/endpoints_context.py:16:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/local/cli_common/invoke_context.py#L62'>samcli/commands/local/cli_common/invoke_context.py:62:7:</a> PLR0902 Too many instance attributes (35/7)
- <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/local/lib/local_api_service.py#L15'>samcli/commands/local/lib/local_api_service.py:15:7:</a> PLR0902 Too many instance attributes (9/7)
... 79 additional changes omitted for rule PLR0902
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/pipeline/bootstrap/guided_context.py#L231'>samcli/commands/pipeline/bootstrap/guided_context.py:231:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/list/endpoints/endpoints_producer.py#L440'>samcli/lib/list/endpoints/endpoints_producer.py:440:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/sync/infra_sync_executor.py#L485'>samcli/lib/sync/infra_sync_executor.py:485:17:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
... 80 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+10 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L27'>release/config.py:27:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L53'>src/bokeh/application/handlers/code_runner.py:53:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L76'>src/bokeh/client/connection.py:76:7:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L92'>src/bokeh/core/property/struct.py:92:17:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L189'>src/bokeh/core/query.py:189:17:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L400'>src/bokeh/core/serialization.py:400:13:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/check.py#L215'>src/bokeh/core/validation/check.py:215:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L86'>src/bokeh/document/callbacks.py:86:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L111'>src/bokeh/document/document.py:111:7:</a> PLR0902 Too many instance attributes (9/7)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L96'>src/bokeh/models/util/structure.py:96:7:</a> PLR0902 Too many instance attributes (10/7)
... 9 additional changes omitted for rule PLR0902
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/journalist_gui/journalist_gui/SecureDropUpdater.py#L169'>journalist_gui/journalist_gui/SecureDropUpdater.py:169:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/journalist_gui/journalist_gui/updaterUI.py#L10'>journalist_gui/journalist_gui/updaterUI.py:10:7:</a> PLR0902 Too many instance attributes (16/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/journalist_app/__init__.py#L127'>securedrop/journalist_app/__init__.py:127:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/journalist_app/sessions.py#L18'>securedrop/journalist_app/sessions.py:18:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/journalist_app/sessions.py#L83'>securedrop/journalist_app/sessions.py:83:7:</a> PLR0902 Too many instance attributes (11/7)
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_meta.py#L110'>securedrop/pretty_bad_protocol/_meta.py:110:7:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_meta.py#L822'>securedrop/pretty_bad_protocol/_meta.py:822:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/freedomofpress/securedrop/blob/bbf143425a1a5b8e9146330baaaf31c104ca7425/securedrop/pretty_bad_protocol/_parsers.py#L1172'>securedrop/pretty_bad_protocol/_parsers.py:1172:7:</a> PLR0902 Too many instance attributes (8/7)
... 5 additional changes omitted for rule PLR0902
... 4 additional changes omitted for project
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
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/bigquery/compiler.py#L332'>ibis/backends/bigquery/compiler.py:332:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/bigquery/udf/core.py#L105'>ibis/backends/bigquery/udf/core.py:105:7:</a> PLR0902 Too many instance attributes (8/7)
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/flink/ddl.py#L77'>ibis/backends/flink/ddl.py:77:7:</a> PLR0902 Too many instance attributes (10/7)
- <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/impala/ddl.py#L73'>ibis/backends/impala/ddl.py:73:7:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/ibis-project/ibis/blob/b5e6373485ffdc56ed5c0232b8d341ef895b62ce/ibis/backends/polars/compiler.py#L149'>ibis/backends/polars/compiler.py:149:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -10 violations, +0 -0 fixes)</summary>
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
... 5 additional changes omitted for rule PLR0902
+ <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/entity_helper.py#L335'>pymilvus/client/entity_helper.py:335:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/client/prepare.py#L408'>pymilvus/client/prepare.py:408:9:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
+ <a href='https://github.com/milvus-io/pymilvus/blob/bfdf7d00cf5ea0d12cde859fe553acc35218c529/pymilvus/orm/schema.py#L59'>pymilvus/orm/schema.py:59:5:</a> PLR5601 Consecutive elif with differing indentation level, consider creating a function to separate the inner elif
... 4 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0902 | 802 | 0 | 802 | 0 | 0 |
| PLR5601 | 161 | 161 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/confusing_consecutive_elif.rs`:54 on 2024-02-25 19:44_

The original pylint docs mention an explicit else as alternative resolution. We should also provide that option I believe.


---

_@sbrugman reviewed on 2024-02-25 19:45_

---

_Label `rule` added by @MichaReiser on 2024-02-26 11:15_

---

_Comment by @codspeed-hq[bot] on 2024-02-27 20:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tibor-reiss:add-PLR5601)

### Merging #10103 will **not alter performance**

<sub>Comparing <code>tibor-reiss:add-PLR5601</code> (5819d6b) with <code>main</code> (8dc22d5)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @MichaReiser on 2024-03-28 16:35_

Thank you, @tibor-reiss, for implementing this rule, and I am sorry that it took us so long to get back to you. 

We're currently revising our rule acceptance criteria as part of the work we do around https://github.com/astral-sh/ruff/issues/1774, and I'm sorry to inform you that we can't accept this rule for now (we can reconsider once https://github.com/astral-sh/ruff/issues/1774 is completed). There are two reasons for it:

1. This is a pylint extension rule. We've accepted pylint extension rules before but we have revised our standpoint because adding them to the pylint group gives users a different experience compared to pylint where they explicitly need to opt in to extension rules. The alternative is to have an explicit rule group for each extension rule but we rather not do that because we don't want 1:1 group to rule associations. We hope to have a better place for pylint extension rules once #1774 is complete. 
2. It's unclear if we'll accept this rule after #1774 is complete because the rule in my view falls into the "It's something someone doesn't like category" and the proposed fix has some clear downsides (performance, reduced readability in my view). Our acceptance guidelines aren't finalized yet, so our stand point on this might change, but I wanted to give you a fair warning ;)



Thank you again for working on the Rule and sorry that I can't give you a more positive feedback

---

_Closed by @MichaReiser on 2024-03-28 16:35_

---

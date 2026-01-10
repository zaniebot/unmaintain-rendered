```yaml
number: 10431
title: "[`pylint`] - implement `too-many-instance-attributes` (`R0902`)"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
base: main
head: add-R0902
created_at: 2024-03-16T20:34:04Z
updated_at: 2024-03-23T03:40:44Z
url: https://github.com/astral-sh/ruff/pull/10431
synced_at: 2026-01-10T22:47:02Z
```

# [`pylint`] - implement `too-many-instance-attributes` (`R0902`)

---

_Pull request opened by @diceroll123 on 2024-03-16 20:34_

## Summary

Adds [`too-many-instance-attributes`](https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/too-many-instance-attributes.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-03-16 20:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+657 -0 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+525 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/dag_processing/manager.py#L355'>airflow/dag_processing/manager.py:355:5:</a> PLR0902 Too many instance attributes (19/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/jobs/backfill_job_runner.py#L114'>airflow/jobs/backfill_job_runner.py:114:5:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/jobs/local_task_job_runner.py#L82'>airflow/jobs/local_task_job_runner.py:82:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/jobs/scheduler_job_runner.py#L154'>airflow/jobs/scheduler_job_runner.py:154:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/baseoperator.py#L749'>airflow/models/baseoperator.py:749:5:</a> PLR0902 Too many instance attributes (36/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/dag.py#L443'>airflow/models/dag.py:443:5:</a> PLR0902 Too many instance attributes (29/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/dagrun.py#L204'>airflow/models/dagrun.py:204:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/taskinstance.py#L1346'>airflow/models/taskinstance.py:1346:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/taskinstance.py#L3534'>airflow/models/taskinstance.py:3534:5:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/models/taskreschedule.py#L84'>airflow/models/taskreschedule.py:84:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/operators/email.py#L51'>airflow/operators/email.py:51:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/operators/trigger_dagrun.py#L107'>airflow/operators/trigger_dagrun.py:107:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/airbyte/operators/airbyte.py#L59'>airflow/providers/airbyte/operators/airbyte.py:59:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/hooks/glue.py#L67'>airflow/providers/amazon/aws/hooks/glue.py:67:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/appflow.py#L75'>airflow/providers/amazon/aws/operators/appflow.py:75:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/athena.py#L89'>airflow/providers/amazon/aws/operators/athena.py:89:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/batch.py#L152'>airflow/providers/amazon/aws/operators/batch.py:152:5:</a> PLR0902 Too many instance attributes (22/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/batch.py#L468'>airflow/providers/amazon/aws/operators/batch.py:468:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/datasync.py#L132'>airflow/providers/amazon/aws/operators/datasync.py:132:5:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/dms.py#L72'>airflow/providers/amazon/aws/operators/dms.py:72:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/ec2.py#L166'>airflow/providers/amazon/aws/operators/ec2.py:166:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/ecs.py#L446'>airflow/providers/amazon/aws/operators/ecs.py:446:5:</a> PLR0902 Too many instance attributes (23/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L219'>airflow/providers/amazon/aws/operators/eks.py:219:5:</a> PLR0902 Too many instance attributes (18/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L478'>airflow/providers/amazon/aws/operators/eks.py:478:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L603'>airflow/providers/amazon/aws/operators/eks.py:603:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L710'>airflow/providers/amazon/aws/operators/eks.py:710:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L841'>airflow/providers/amazon/aws/operators/eks.py:841:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eks.py#L931'>airflow/providers/amazon/aws/operators/eks.py:931:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L1052'>airflow/providers/amazon/aws/operators/emr.py:1052:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L108'>airflow/providers/amazon/aws/operators/emr.py:108:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L1280'>airflow/providers/amazon/aws/operators/emr.py:1280:5:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L252'>airflow/providers/amazon/aws/operators/emr.py:252:5:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L534'>airflow/providers/amazon/aws/operators/emr.py:534:5:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/emr.py#L726'>airflow/providers/amazon/aws/operators/emr.py:726:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/eventbridge.py#L126'>airflow/providers/amazon/aws/operators/eventbridge.py:126:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/glue.py#L88'>airflow/providers/amazon/aws/operators/glue.py:88:5:</a> PLR0902 Too many instance attributes (22/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/lambda_function.py#L77'>airflow/providers/amazon/aws/operators/lambda_function.py:77:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/apache/airflow/blob/bf137f874bdb8ab1257211f13fce1cc00c85f399/airflow/providers/amazon/aws/operators/rds.py#L192'>airflow/providers/amazon/aws/operators/rds.py:192:5:</a> PLR0902 Too many instance attributes (11/7)
... 486 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/build/build_context.py#L54'>samcli/commands/build/build_context.py:54:5:</a> PLR0902 Too many instance attributes (26/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/delete/delete_context.py#L32'>samcli/commands/delete/delete_context.py:32:5:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/deploy/deploy_context.py#L51'>samcli/commands/deploy/deploy_context.py:51:5:</a> PLR0902 Too many instance attributes (27/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/deploy/guided_context.py#L44'>samcli/commands/deploy/guided_context.py:44:5:</a> PLR0902 Too many instance attributes (32/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/list/endpoints/endpoints_context.py#L21'>samcli/commands/list/endpoints/endpoints_context.py:21:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/local/cli_common/invoke_context.py#L78'>samcli/commands/local/cli_common/invoke_context.py:78:5:</a> PLR0902 Too many instance attributes (23/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/local/lib/local_api_service.py#L21'>samcli/commands/local/lib/local_api_service.py:21:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/local/lib/local_lambda.py#L43'>samcli/commands/local/lib/local_lambda.py:43:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/package/package_context.py#L58'>samcli/commands/package/package_context.py:58:5:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/aws/aws-sam-cli/blob/2f5d408c7020ffa5b9272289beb0fbcad4bb5824/samcli/commands/pipeline/bootstrap/guided_context.py#L48'>samcli/commands/pipeline/bootstrap/guided_context.py:48:5:</a> PLR0902 Too many instance attributes (16/7)
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L72'>src/bokeh/application/handlers/code_runner.py:72:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L85'>src/bokeh/client/connection.py:85:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L109'>src/bokeh/document/callbacks.py:109:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L134'>src/bokeh/document/document.py:134:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L330'>src/bokeh/resources.py:330:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L156'>src/bokeh/server/contexts.py:156:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L131'>src/bokeh/server/session.py:131:5:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L253'>src/bokeh/server/tornado.py:253:5:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L72'>src/bokeh/server/views/ws.py:72:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L372'>src/bokeh/settings.py:372:5:</a> PLR0902 Too many instance attributes (8/7)
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_meta.py#L135'>securedrop/pretty_bad_protocol/_meta.py:135:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1187'>securedrop/pretty_bad_protocol/_parsers.py:1187:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1506'>securedrop/pretty_bad_protocol/_parsers.py:1506:5:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L985'>securedrop/pretty_bad_protocol/_parsers.py:985:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/upload-screenshots.py#L78'>securedrop/upload-screenshots.py:78:5:</a> PLR0902 Too many instance attributes (9/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/blinkpy/auth.py#L25'>blinkpy/auth.py:25:5:</a> PLR0902 Too many instance attributes (12/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/blinkpy/blinkpy.py#L44'>blinkpy/blinkpy.py:44:5:</a> PLR0902 Too many instance attributes (17/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/blinkpy/camera.py#L22'>blinkpy/camera.py:22:5:</a> PLR0902 Too many instance attributes (23/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/blinkpy/sync_module.py#L26'>blinkpy/sync_module.py:26:5:</a> PLR0902 Too many instance attributes (21/7)
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/tests/mock_responses.py#L8'>tests/mock_responses.py:8:5:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/8b98db78f6459759ed28790c7aae59af05b84f8c/ibis/backends/bigquery/udf/core.py#L120'>ibis/backends/bigquery/udf/core.py:120:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/ibis-project/ibis/blob/8b98db78f6459759ed28790c7aae59af05b84f8c/ibis/backends/flink/ddl.py#L78'>ibis/backends/flink/ddl.py:78:5:</a> PLR0902 Too many instance attributes (10/7)
+ <a href='https://github.com/ibis-project/ibis/blob/8b98db78f6459759ed28790c7aae59af05b84f8c/ibis/backends/impala/ddl.py#L74'>ibis/backends/impala/ddl.py:74:5:</a> PLR0902 Too many instance attributes (8/7)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/bulk_writer/bulk_writer.py#L37'>pymilvus/bulk_writer/bulk_writer.py:37:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/bulk_writer/remote_bulk_writer.py#L39'>pymilvus/bulk_writer/remote_bulk_writer.py:39:9:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/client/abstract.py#L105'>pymilvus/client/abstract.py:105:5:</a> PLR0902 Too many instance attributes (14/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/client/abstract.py#L16'>pymilvus/client/abstract.py:16:5:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/client/abstract.py#L189'>pymilvus/client/abstract.py:189:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/client/asynch.py#L57'>pymilvus/client/asynch.py:57:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/model/hybrid/bge_m3.py#L14'>pymilvus/model/hybrid/bge_m3.py:14:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/model/sparse/bm25/bm25.py#L39'>pymilvus/model/sparse/bm25/bm25.py:39:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/model/sparse/splade.py#L45'>pymilvus/model/sparse/splade.py:45:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/milvus-io/pymilvus/blob/436f559a15317a4dbeab68d74176f3fc588700af/pymilvus/orm/iterator.py#L282'>pymilvus/orm/iterator.py:282:5:</a> PLR0902 Too many instance attributes (9/7)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/apply.py#L116'>pandas/core/apply.py:116:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/groupby/groupby.py#L1093'>pandas/core/groupby/groupby.py:1093:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/groupby/grouper.py#L252'>pandas/core/groupby/grouper.py:252:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/groupby/grouper.py#L432'>pandas/core/groupby/grouper.py:432:5:</a> PLR0902 Too many instance attributes (11/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/resample.py#L1480'>pandas/core/resample.py:1480:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/resample.py#L162'>pandas/core/resample.py:162:5:</a> PLR0902 Too many instance attributes (13/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/resample.py#L1922'>pandas/core/resample.py:1922:5:</a> PLR0902 Too many instance attributes (8/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/reshape/concat.py#L388'>pandas/core/reshape/concat.py:388:5:</a> PLR0902 Too many instance attributes (9/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/reshape/merge.py#L732'>pandas/core/reshape/merge.py:732:5:</a> PLR0902 Too many instance attributes (16/7)
+ <a href='https://github.com/pandas-dev/pandas/blob/3267ba06f17b40bef359999dbbd8f3a424a65a89/pandas/core/reshape/reshape.py#L113'>pandas/core/reshape/reshape.py:113:5:</a> PLR0902 Too many instance attributes (10/7)
... 29 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0902 | 657 | 657 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-03-20 14:24_

Hey @diceroll123 this looks like a duplicate of https://github.com/astral-sh/ruff/pull/9844, does your approach differ in a substantive way?

---

_Comment by @diceroll123 on 2024-03-23 03:40_

Woop, didn't notice there was a PR already!

I concede to the other PR. 🙏🏽 

---

_Closed by @diceroll123 on 2024-03-23 03:40_

---

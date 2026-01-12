```yaml
number: 11321
title: Consider with statements for too many branches lint
type: pull_request
state: merged
author: blueraft
labels:
  - bug
assignees: []
merged: true
base: main
head: with-statements-too-many-branches
created_at: 2024-05-07T10:19:51Z
updated_at: 2024-05-08T08:19:55Z
url: https://github.com/astral-sh/ruff/pull/11321
synced_at: 2026-01-12T15:55:37Z
```

# Consider with statements for too many branches lint

---

_@blueraft_

Resolves https://github.com/astral-sh/ruff/issues/11313

## Summary

PLR0912(too-many-branches) did not count branches inside with: blocks. With this fix, the branches inside with statements are also counted.

## Test Plan

Added a new test case.


---

_Comment by @github-actions[bot] on 2024-05-07 10:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+45 -16 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+29 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (19 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L404'>airflow/cli/commands/task_command.py:404:5:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L404'>airflow/cli/commands/task_command.py:404:5:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L631'>airflow/cli/commands/task_command.py:631:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L631'>airflow/cli/commands/task_command.py:631:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/dagrun.py#L755'>airflow/models/dagrun.py:755:9:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L2489'>airflow/models/taskinstance.py:2489:9:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L400'>airflow/models/taskinstance.py:400:5:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L400'>airflow/models/taskinstance.py:400:5:</a> PLR0912 Too many branches (22 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/apache/hive/transfers/s3_to_hive.py#L141'>airflow/providers/apache/hive/transfers/s3_to_hive.py:141:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/apache/livy/hooks/livy.py#L496'>airflow/providers/apache/livy/hooks/livy.py:496:15:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L213'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:213:9:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L379'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:379:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/common/sql/hooks/sql.py#L356'>airflow/providers/common/sql/hooks/sql.py:356:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/databricks/hooks/databricks_sql.py#L199'>airflow/providers/databricks/hooks/databricks_sql.py:199:9:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/databricks/operators/databricks_sql.py#L126'>airflow/providers/databricks/operators/databricks_sql.py:126:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/elasticsearch/log/es_task_handler.py#L216'>airflow/providers/elasticsearch/log/es_task_handler.py:216:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/exasol/hooks/exasol.py#L187'>airflow/providers/exasol/hooks/exasol.py:187:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/google/cloud/operators/gcs.py#L792'>airflow/providers/google/cloud/operators/gcs.py:792:9:</a> PLR0912 Too many branches (20 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/http/hooks/http.py#L318'>airflow/providers/http/hooks/http.py:318:15:</a> PLR0912 Too many branches (24 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/openlineage/utils/utils.py#L305'>airflow/providers/openlineage/utils/utils.py:305:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/snowflake/hooks/snowflake.py#L340'>airflow/providers/snowflake/hooks/snowflake.py:340:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/utils/db.py#L1554'>airflow/utils/db.py:1554:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/www/views.py#L3424'>airflow/www/views.py:3424:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/www/views.py#L739'>airflow/www/views.py:739:9:</a> PLR0912 Too many branches (36 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2021'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2021:5:</a> PLR0912 Too many branches (23 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3277'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3277:5:</a> PLR0912 Too many branches (19 > 12)
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L802'>src/bokeh/command/subcommands/serve.py:802:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L802'>src/bokeh/command/subcommands/serve.py:802:9:</a> PLR0912 Too many branches (31 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (17 > 12)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+14 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L1700'>corporate/lib/stripe.py:1700:9:</a> PLR0912 Too many branches (19 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L2770'>corporate/lib/stripe.py:2770:9:</a> PLR0912 Too many branches (27 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L2770'>corporate/lib/stripe.py:2770:9:</a> PLR0912 Too many branches (28 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/create_realm.py#L155'>zerver/actions/create_realm.py:155:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/create_realm.py#L155'>zerver/actions/create_realm.py:155:5:</a> PLR0912 Too many branches (22 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/message_send.py#L846'>zerver/actions/message_send.py:846:5:</a> PLR0912 Too many branches (29 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/message_send.py#L846'>zerver/actions/message_send.py:846:5:</a> PLR0912 Too many branches (34 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/scheduled_messages.py#L129'>zerver/actions/scheduled_messages.py:129:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L775'>zerver/lib/import_realm.py:775:5:</a> PLR0912 Too many branches (32 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L775'>zerver/lib/import_realm.py:775:5:</a> PLR0912 Too many branches (33 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L966'>zerver/lib/import_realm.py:966:5:</a> PLR0912 Too many branches (38 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L966'>zerver/lib/import_realm.py:966:5:</a> PLR0912 Too many branches (48 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/push_notifications.py#L1267'>zerver/lib/push_notifications.py:1267:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/management/commands/export_search.py#L101'>zerver/management/commands/export_search.py:101:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/management/commands/export_search.py#L101'>zerver/management/commands/export_search.py:101:9:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/views/message_fetch.py#L99'>zerver/views/message_fetch.py:99:5:</a> PLR0912 Too many branches (22 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/views/streams.py#L699'>zerver/views/streams.py:699:5:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/worker/base.py#L166'>zerver/worker/base.py:166:9:</a> PLR0912 Too many branches (14 > 12)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0912 | 61 | 45 | 16 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+45 -16 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+29 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (19 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L404'>airflow/cli/commands/task_command.py:404:5:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L404'>airflow/cli/commands/task_command.py:404:5:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L631'>airflow/cli/commands/task_command.py:631:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/cli/commands/task_command.py#L631'>airflow/cli/commands/task_command.py:631:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/dagrun.py#L755'>airflow/models/dagrun.py:755:9:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L2489'>airflow/models/taskinstance.py:2489:9:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L400'>airflow/models/taskinstance.py:400:5:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/models/taskinstance.py#L400'>airflow/models/taskinstance.py:400:5:</a> PLR0912 Too many branches (22 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/apache/hive/transfers/s3_to_hive.py#L141'>airflow/providers/apache/hive/transfers/s3_to_hive.py:141:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/apache/livy/hooks/livy.py#L496'>airflow/providers/apache/livy/hooks/livy.py:496:15:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L213'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:213:9:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L379'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:379:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/common/sql/hooks/sql.py#L356'>airflow/providers/common/sql/hooks/sql.py:356:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/databricks/hooks/databricks_sql.py#L199'>airflow/providers/databricks/hooks/databricks_sql.py:199:9:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/databricks/operators/databricks_sql.py#L126'>airflow/providers/databricks/operators/databricks_sql.py:126:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/elasticsearch/log/es_task_handler.py#L216'>airflow/providers/elasticsearch/log/es_task_handler.py:216:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/exasol/hooks/exasol.py#L187'>airflow/providers/exasol/hooks/exasol.py:187:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/google/cloud/operators/gcs.py#L792'>airflow/providers/google/cloud/operators/gcs.py:792:9:</a> PLR0912 Too many branches (20 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/http/hooks/http.py#L318'>airflow/providers/http/hooks/http.py:318:15:</a> PLR0912 Too many branches (24 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/openlineage/utils/utils.py#L305'>airflow/providers/openlineage/utils/utils.py:305:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/providers/snowflake/hooks/snowflake.py#L340'>airflow/providers/snowflake/hooks/snowflake.py:340:9:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/utils/db.py#L1554'>airflow/utils/db.py:1554:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/www/views.py#L3424'>airflow/www/views.py:3424:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/airflow/www/views.py#L739'>airflow/www/views.py:739:9:</a> PLR0912 Too many branches (36 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2021'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2021:5:</a> PLR0912 Too many branches (23 > 12)
+ <a href='https://github.com/apache/airflow/blob/f5c86edb7967102d26435018e367354735044f56/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3277'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3277:5:</a> PLR0912 Too many branches (19 > 12)
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L802'>src/bokeh/command/subcommands/serve.py:802:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L802'>src/bokeh/command/subcommands/serve.py:802:9:</a> PLR0912 Too many branches (31 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (15 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (17 > 12)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+14 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L1700'>corporate/lib/stripe.py:1700:9:</a> PLR0912 Too many branches (19 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L2770'>corporate/lib/stripe.py:2770:9:</a> PLR0912 Too many branches (27 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/corporate/lib/stripe.py#L2770'>corporate/lib/stripe.py:2770:9:</a> PLR0912 Too many branches (28 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/create_realm.py#L155'>zerver/actions/create_realm.py:155:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/create_realm.py#L155'>zerver/actions/create_realm.py:155:5:</a> PLR0912 Too many branches (22 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/message_send.py#L846'>zerver/actions/message_send.py:846:5:</a> PLR0912 Too many branches (29 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/message_send.py#L846'>zerver/actions/message_send.py:846:5:</a> PLR0912 Too many branches (34 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/actions/scheduled_messages.py#L129'>zerver/actions/scheduled_messages.py:129:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L775'>zerver/lib/import_realm.py:775:5:</a> PLR0912 Too many branches (32 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L775'>zerver/lib/import_realm.py:775:5:</a> PLR0912 Too many branches (33 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L966'>zerver/lib/import_realm.py:966:5:</a> PLR0912 Too many branches (38 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/import_realm.py#L966'>zerver/lib/import_realm.py:966:5:</a> PLR0912 Too many branches (48 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/lib/push_notifications.py#L1267'>zerver/lib/push_notifications.py:1267:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/management/commands/export_search.py#L101'>zerver/management/commands/export_search.py:101:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/management/commands/export_search.py#L101'>zerver/management/commands/export_search.py:101:9:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/views/message_fetch.py#L99'>zerver/views/message_fetch.py:99:5:</a> PLR0912 Too many branches (22 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/views/streams.py#L699'>zerver/views/streams.py:699:5:</a> PLR0912 Too many branches (14 > 12)
+ <a href='https://github.com/zulip/zulip/blob/a7022bdfecfa82436dfc87f28c6713cbe8b25006/zerver/worker/base.py#L166'>zerver/worker/base.py:166:9:</a> PLR0912 Too many branches (14 > 12)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0912 | 61 | 45 | 16 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 10:35_

I think it should include the `with` statement _and_ the number of branches in its body.

```suggestion
            Stmt::With(ast::StmtWith { body, .. }) => 1 + num_branches(body),
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:1 on 2024-05-07 10:36_

Can you include additional test cases in this file for `with` statements? They will be at the bottom of this file and they specifically tests the `num_branches` function. 

---

_@dhruvmanila requested changes on 2024-05-07 10:36_

---

_Label `bug` added by @dhruvmanila on 2024-05-07 10:36_

---

_@MichaReiser reviewed on 2024-05-07 10:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 10:47_

I think the number of branches is sufficient because the with item itself doesn't introduce any branching.

---

_@dhruvmanila reviewed on 2024-05-07 11:01_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 11:01_

The count would then be inconsistent with other branch constructs which _do_ include the statement itself in the final count.

---

_@blueraft reviewed on 2024-05-07 11:03_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 11:03_

For the python file below, I get the same amount of branches in both cases from pylint. Seems like pylint doesn't include the `with` statement.

```bash
❯ pylint bad.py
************* Module bad
bad.py:6:8: R1705: Unnecessary "elif" after "return", remove the leading "el" from "elif" (no-else-return)
bad.py:4:0: R0912: Too many branches (11/10) (too-many-branches)
bad.py:31:4: R1705: Unnecessary "elif" after "return", remove the leading "el" from "elif" (no-else-return)
bad.py:30:0: R0912: Too many branches (11/10) (too-many-branches)
```

```python
from contextlib import suppress


def num_to_word_with(x):  # [too-many-branches]
    with suppress(Exception):
        if x == 0:
            return 'zero'
        elif x == 1:
            return 'one'
        elif x == 2:
            return 'two'
        elif x == 3:
            return 'three'
        elif x == 4:
            return 'four'
        elif x == 5:
            return 'five'
        elif x == 6:
            return 'six'
        elif x == 7:
            return 'seven'
        elif x == 8:
            return 'eight'
        elif x == 9:
            return 'nine'
        else:
            return None


def num_to_word(x):  # [too-many-branches]
    if x == 0:
        return 'zero'
    elif x == 1:
        return 'one'
    elif x == 2:
        return 'two'
    elif x == 3:
        return 'three'
    elif x == 4:
        return 'four'
    elif x == 5:
        return 'five'
    elif x == 6:
        return 'six'
    elif x == 7:
        return 'seven'
    elif x == 8:
        return 'eight'
    elif x == 9:
        return 'nine'
    else:
        return None
```


---

_@MichaReiser reviewed on 2024-05-07 11:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 11:04_

> The count would then be inconsistent with other branch constructs which do include the statement itself in the final count.

I think that inconsistency is desired because with items don't introduce a new branch. They are not branching instructions. 


---

_@dhruvmanila reviewed on 2024-05-07 11:45_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:110 on 2024-05-07 11:45_

Oh yeah, that makes sense. I'll mark this as resolved then. Thanks @blueraft for looking into pylint.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:282 on 2024-05-07 11:46_

Thanks for clarifying this with a comment but I think it would be better suited where the original code is in `num_branches`.

---

_@dhruvmanila approved on 2024-05-07 11:46_

---

_Comment by @dhruvmanila on 2024-05-07 11:47_

Thank you for working on this! Welcome to Ruff :)

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:282 on 2024-05-07 11:52_

I've moved it :) 

---

_@blueraft reviewed on 2024-05-07 11:52_

---

_Merged by @charliermarsh on 2024-05-08 03:10_

---

_Closed by @charliermarsh on 2024-05-08 03:10_

---

_Branch deleted on 2024-05-08 08:19_

---

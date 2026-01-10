```yaml
number: 13613
title: "Mark `FURB118` fix as unsafe"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: zb/reimpl-op-unsafe
created_at: 2024-10-03T17:25:39Z
updated_at: 2024-10-03T21:48:30Z
url: https://github.com/astral-sh/ruff/pull/13613
synced_at: 2026-01-10T20:59:36Z
```

# Mark `FURB118` fix as unsafe

---

_Pull request opened by @zanieb on 2024-10-03 17:25_

Closes https://github.com/astral-sh/ruff/issues/13421


---

_Label `fixes` added by @zanieb on 2024-10-03 17:25_

---

_Comment by @github-actions[bot] on 2024-10-03 17:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -232 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -88 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/api_connexion/endpoints/task_instance_endpoint.py#L778'>airflow/api_connexion/endpoints/task_instance_endpoint.py:778:25:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/api_connexion/endpoints/task_instance_endpoint.py#L778'>airflow/api_connexion/endpoints/task_instance_endpoint.py:778:25:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L138'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:138:28:</a> FURB118 Use `operator.itemgetter("SnapshotCreateTime")` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L138'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:138:28:</a> FURB118 [*] Use `operator.itemgetter("SnapshotCreateTime")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/databricks/operators/databricks.py#L1126'>airflow/providers/databricks/operators/databricks.py:1126:46:</a> FURB118 Use `operator.itemgetter("start_time")` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/databricks/operators/databricks.py#L1126'>airflow/providers/databricks/operators/databricks.py:1126:46:</a> FURB118 [*] Use `operator.itemgetter("start_time")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/weaviate/hooks/weaviate.py#L638'>airflow/providers/weaviate/hooks/weaviate.py:638:31:</a> FURB118 Use `operator.itemgetter(uuid_column)` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/weaviate/hooks/weaviate.py#L638'>airflow/providers/weaviate/hooks/weaviate.py:638:31:</a> FURB118 [*] Use `operator.itemgetter(uuid_column)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/weaviate/hooks/weaviate.py#L682'>airflow/providers/weaviate/hooks/weaviate.py:682:23:</a> FURB118 Use `operator.itemgetter(uuid_column)` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers/weaviate/hooks/weaviate.py#L682'>airflow/providers/weaviate/hooks/weaviate.py:682:23:</a> FURB118 [*] Use `operator.itemgetter(uuid_column)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers_manager.py#L1347'>airflow/providers_manager.py:1347:59:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers_manager.py#L1347'>airflow/providers_manager.py:1347:59:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers_manager.py#L1351'>airflow/providers_manager.py:1351:59:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/airflow/providers_manager.py#L1351'>airflow/providers_manager.py:1351:59:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/dev/stats/get_important_pr_candidates.py#L387'>dev/stats/get_important_pr_candidates.py:387:69:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/dev/stats/get_important_pr_candidates.py#L387'>dev/stats/get_important_pr_candidates.py:387:69:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/docs/conf.py#L573'>docs/conf.py:573:26:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a lambda
- <a href='https://github.com/apache/airflow/blob/9ec21405c0abe23f6de2e0acd704faa6631fbe76/docs/conf.py#L573'>docs/conf.py:573:26:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -32 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/scripts/cancel_github_workflows.py#L205'>scripts/cancel_github_workflows.py:205:29:</a> FURB118 Use `operator.itemgetter("created_at")` instead of defining a lambda
- <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/scripts/cancel_github_workflows.py#L205'>scripts/cancel_github_workflows.py:205:29:</a> FURB118 [*] Use `operator.itemgetter("created_at")` instead of defining a lambda
+ <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/charts/post_processing.py#L234'>superset/charts/post_processing.py:234:14:</a> FURB118 Use `operator.itemgetter(slice(1))` instead of defining a lambda
- <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/charts/post_processing.py#L234'>superset/charts/post_processing.py:234:14:</a> FURB118 [*] Use `operator.itemgetter(slice(1))` instead of defining a lambda
+ <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/charts/post_processing.py#L235'>superset/charts/post_processing.py:235:13:</a> FURB118 Use `operator.itemgetter(slice(-1, None))` instead of defining a lambda
- <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/charts/post_processing.py#L235'>superset/charts/post_processing.py:235:13:</a> FURB118 [*] Use `operator.itemgetter(slice(-1, None))` instead of defining a lambda
+ <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/commands/database/tables.py#L125'>superset/commands/database/tables.py:125:21:</a> FURB118 Use `operator.itemgetter("value")` instead of defining a lambda
- <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/commands/database/tables.py#L125'>superset/commands/database/tables.py:125:21:</a> FURB118 [*] Use `operator.itemgetter("value")` instead of defining a lambda
+ <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/databases/api.py#L2101'>superset/databases/api.py:2101:21:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a lambda
- <a href='https://github.com/apache/superset/blob/4dfee727e8e6d3b519788c9c035320f7b4054343/superset/databases/api.py#L2101'>superset/databases/api.py:2101:21:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -26 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/les_mis.py#L18'>examples/topics/categorical/les_mis.py:18:61:</a> FURB118 Use `operator.itemgetter('group')` instead of defining a lambda
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/les_mis.py#L18'>examples/topics/categorical/les_mis.py:18:61:</a> FURB118 [*] Use `operator.itemgetter('group')` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L354'>src/bokeh/core/query.py:354:9:</a> FURB118 Use `operator.gt` instead of defining a lambda
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L354'>src/bokeh/core/query.py:354:9:</a> FURB118 [*] Use `operator.gt` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L355'>src/bokeh/core/query.py:355:9:</a> FURB118 Use `operator.lt` instead of defining a lambda
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L355'>src/bokeh/core/query.py:355:9:</a> FURB118 [*] Use `operator.lt` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L356'>src/bokeh/core/query.py:356:9:</a> FURB118 Use `operator.eq` instead of defining a lambda
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L356'>src/bokeh/core/query.py:356:9:</a> FURB118 [*] Use `operator.eq` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L357'>src/bokeh/core/query.py:357:9:</a> FURB118 Use `operator.ge` instead of defining a lambda
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L357'>src/bokeh/core/query.py:357:9:</a> FURB118 [*] Use `operator.ge` instead of defining a lambda
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -86 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/analytics/views/stats.py#L570'>analytics/views/stats.py:570:82:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/analytics/views/stats.py#L570'>analytics/views/stats.py:570:82:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/corporate/views/support.py#L608'>corporate/views/support.py:608:39:</a> FURB118 Use `operator.itemgetter("display_order")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/corporate/views/support.py#L608'>corporate/views/support.py:608:39:</a> FURB118 [*] Use `operator.itemgetter("display_order")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/default_streams.py#L195'>zerver/actions/default_streams.py:195:62:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/default_streams.py#L195'>zerver/actions/default_streams.py:195:62:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/message_send.py#L425'>zerver/actions/message_send.py:425:9:</a> FURB118 Use `operator.itemgetter("enable_online_push_notifications")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/message_send.py#L425'>zerver/actions/message_send.py:425:9:</a> FURB118 [*] Use `operator.itemgetter("enable_online_push_notifications")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/message_send.py#L444'>zerver/actions/message_send.py:444:9:</a> FURB118 Use `operator.itemgetter("long_term_idle")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/actions/message_send.py#L444'>zerver/actions/message_send.py:444:9:</a> FURB118 [*] Use `operator.itemgetter("long_term_idle")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/context_processors.py#L295'>zerver/context_processors.py:295:68:</a> FURB118 Use `operator.itemgetter("display_order")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/context_processors.py#L295'>zerver/context_processors.py:295:68:</a> FURB118 [*] Use `operator.itemgetter("display_order")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/default_streams.py#L30'>zerver/lib/default_streams.py:30:37:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/default_streams.py#L30'>zerver/lib/default_streams.py:30:37:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/display_recipient.py#L143'>zerver/lib/display_recipient.py:143:24:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/display_recipient.py#L143'>zerver/lib/display_recipient.py:143:24:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/display_recipient.py#L144'>zerver/lib/display_recipient.py:144:16:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a lambda
- <a href='https://github.com/zulip/zulip/blob/1b47134d0d564f8ba4961d25743f3ad0f09e6dfb/zerver/lib/display_recipient.py#L144'>zerver/lib/display_recipient.py:144:16:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
... 68 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 232 | 0 | 0 | 0 | 232 |

</p>
</details>




---

_Label `bug` added by @zanieb on 2024-10-03 17:54_

---

_Label `preview` added by @zanieb on 2024-10-03 17:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:49 on 2024-10-03 20:33_

```suggestion
/// `operator.add`, will cause the call to raise a `TypeError`, as `operator`-module functions do not allow
```

---

_@AlexWaygood approved on 2024-10-03 20:33_

---

_@zanieb reviewed on 2024-10-03 21:27_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:49 on 2024-10-03 21:27_

Maybe?
```suggestion
/// `operator.add`, will cause the call to raise a `TypeError`, as functions in `operator` do not allow
```

---

_@AlexWaygood reviewed on 2024-10-03 21:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:49 on 2024-10-03 21:33_

Sure, fine by me!

---

_Merged by @zanieb on 2024-10-03 21:39_

---

_Closed by @zanieb on 2024-10-03 21:39_

---

_Branch deleted on 2024-10-03 21:39_

---

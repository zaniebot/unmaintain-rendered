```yaml
number: 5767
title: "Expand `RUF015` to include all expression types"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ruf015
created_at: 2023-07-14T23:37:51Z
updated_at: 2023-07-21T00:08:09Z
url: https://github.com/astral-sh/ruff/pull/5767
synced_at: 2026-01-12T03:30:21Z
```

# Expand `RUF015` to include all expression types

---

_Pull request opened by @charliermarsh on 2023-07-14 23:37_

## Summary

We now allow RUF015 to fix cases like:

```python
list(range(10))[0]
list(x.y)[0]
list(x["y"])[0]
```

Further, we fix generators like:

```python
[i + 1 for i in x][0]
```

By rewriting to `next(iter(i + 1 for i in x))`.

I've retained the special-case that rewrites `[i for i in x][0]` to `next(iter(x))`.

Closes https://github.com/astral-sh/ruff/issues/5764.


---

_Comment by @github-actions[bot] on 2023-07-14 23:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+72, -0, 0 error(s))

<details><summary>airflow (+25, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/airflow/providers/amazon/aws/secrets/secrets_manager.py#L221'>airflow/providers/amazon/aws/secrets/secrets_manager.py:221:38:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/airflow/providers/apache/hive/macros/hive.py#L102'>airflow/providers/apache/hive/macros/hive.py:102:18:</a> RUF015 [*] Prefer `next(iter(p.values()))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/airflow/providers/cncf/kubernetes/triggers/pod.py#L249'>airflow/providers/cncf/kubernetes/triggers/pod.py:249:21:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/airflow/providers/google/cloud/operators/datafusion.py#L47'>airflow/providers/google/cloud/operators/datafusion.py:47:22:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/airflow/serialization/serialized_objects.py#L1136'>airflow/serialization/serialized_objects.py:1136:47:</a> RUF015 [*] Prefer `next(iter(_operator_links_source.items()))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/dev/perf/scheduler_dag_execution_timing.py#L83'>dev/perf/scheduler_dag_execution_timing.py:83:19:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/kubernetes_tests/test_kubernetes_pod_operator.py#L927'>kubernetes_tests/test_kubernetes_pod_operator.py:927:22:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/scripts/ci/pre_commit/pre_commit_version_heads_map.py#L39'>scripts/ci/pre_commit/pre_commit_version_heads_map.py:39:30:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/decorators/test_python.py#L465'>tests/decorators/test_python.py:465:22:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/executors/test_kubernetes_executor.py#L438'>tests/executors/test_kubernetes_executor.py:438:20:</a> RUF015 [*] Prefer `next(iter(executor.event_buffer.values()))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/jobs/test_triggerer_job.py#L556'>tests/jobs/test_triggerer_job.py:556:35:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/models/test_dagrun.py#L1709'>tests/models/test_dagrun.py:1709:11:</a> RUF015 [*] Prefer `next(i for i in tis if i.map_index == 0)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/operators/test_python.py#L1184'>tests/operators/test_python.py:1184:32:</a> RUF015 [*] Prefer `next(x for x in tis if x.task_id == "op1")` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/operators/test_python.py#L1215'>tests/operators/test_python.py:1215:32:</a> RUF015 [*] Prefer `next(x for x in tis if x.task_id == "op1")` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/operators/test_python.py#L1242'>tests/operators/test_python.py:1242:32:</a> RUF015 [*] Prefer `next(x for x in tis if x.task_id == "op1")` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/operators/test_python.py#L1277'>tests/operators/test_python.py:1277:32:</a> RUF015 [*] Prefer `next(x for x in tis if x.task_id == "op1")` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/providers/amazon/aws/secrets/test_systems_manager.py#L160'>tests/providers/amazon/aws/secrets/test_systems_manager.py:160:27:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/providers/google/cloud/operators/test_bigquery.py#L678'>tests/providers/google/cloud/operators/test_bigquery.py:678:27:</a> RUF015 [*] Prefer `next(iter(simple_task.operator_extra_links))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/providers/google/cloud/operators/test_bigquery.py#L716'>tests/providers/google/cloud/operators/test_bigquery.py:716:27:</a> RUF015 [*] Prefer `next(iter(simple_task.operator_extra_links))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/providers/qubole/operators/test_qubole.py#L156'>tests/providers/qubole/operators/test_qubole.py:156:27:</a> RUF015 [*] Prefer `next(iter(simple_task.operator_extra_links))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/system/providers/amazon/aws/utils/__init__.py#L67'>tests/system/providers/amazon/aws/utils/__init__.py:67:26:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/test_utils/providers.py#L56'>tests/test_utils/providers.py:56:19:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/www/test_utils.py#L436'>tests/www/test_utils.py:436:15:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/www/views/test_views_acl.py#L339'>tests/www/views/test_views_acl.py:339:16:</a> RUF015 [*] Prefer `next(iter(resp.json.items()))` over single element slice
+ <a href='https://github.com/apache/airflow/blob/848c69a194c03ed3a5badc909e26b5c1bda03050/tests/www/views/test_views_base.py#L254'>tests/www/views/test_views_base.py:254:16:</a> RUF015 [*] Prefer `next(iter(resp.json.items()))` over single element slice
</pre>

</p>
</details>
<details><summary>bokeh (+7, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/core/enums.py#L362'>src/bokeh/core/enums.py:362:41:</a> RUF015 [*] Prefer `next(iter(zip(*_hatch_patterns)))` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/embed/standalone.py#L125'>src/bokeh/embed/standalone.py:125:22:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/io/notebook.py#L110'>src/bokeh/io/notebook.py:110:24:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/plotting/_legends.py#L55'>src/bokeh/plotting/_legends.py:55:20:</a> RUF015 [*] Prefer `next(iter(legend_kwarg.items()))` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/unit/bokeh/embed/test_standalone.py#L412'>tests/unit/bokeh/embed/test_standalone.py:412:20:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/unit/bokeh/embed/test_util__embed.py#L512'>tests/unit/bokeh/embed/test_util__embed.py:512:15:</a> RUF015 [*] Prefer `next(iter(docs_json.values()))` over single element slice
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/unit/bokeh/embed/test_util__embed.py#L524'>tests/unit/bokeh/embed/test_util__embed.py:524:15:</a> RUF015 [*] Prefer `next(iter(docs_json.values()))` over single element slice
</pre>

</p>
</details>
<details><summary>scikit-build (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/bd8802fb7cbf04e354d0c841efed3c0ee6eff448/skbuild/cmaker.py#L753'>skbuild/cmaker.py:753:20:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/scikit-build/scikit-build/blob/bd8802fb7cbf04e354d0c841efed3c0ee6eff448/skbuild/command/egg_info.py#L27'>skbuild/command/egg_info.py:27:32:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/scikit-build/scikit-build/blob/bd8802fb7cbf04e354d0c841efed3c0ee6eff448/skbuild/command/egg_info.py#L28'>skbuild/command/egg_info.py:28:41:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/scikit-build/scikit-build/blob/bd8802fb7cbf04e354d0c841efed3c0ee6eff448/tests/test_setup.py#L778'>tests/test_setup.py:778:22:</a> RUF015 [*] Prefer `next(...)` over single element slice
</pre>

</p>
</details>
<details><summary>zulip (+36, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/analytics/management/commands/populate_analytics_db.py#L158'>analytics/management/commands/populate_analytics_db.py:158:67:</a> RUF015 [*] Prefer `next(iter(fixture_data.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/corporate/views/portico.py#L136'>corporate/views/portico.py:136:37:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/tools/tests/test_zulint_custom_rules.py#L71'>tools/tests/test_zulint_custom_rules.py:71:32:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/event_schema.py#L458'>zerver/lib/event_schema.py:458:12:</a> RUF015 [*] Prefer `next(iter(event["presence"].keys()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/event_schema.py#L460'>zerver/lib/event_schema.py:460:12:</a> RUF015 [*] Prefer `next(iter(event["presence"].values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/markdown/__init__.py#L1155'>zerver/lib/markdown/__init__.py:1155:26:</a> RUF015 [*] Prefer `next(iter(uncle.iter(tag="a")))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/test_classes.py#L1545'>zerver/lib/test_classes.py:1545:16:</a> RUF015 [*] Prefer `next(r for r in data if r["id"] == db_id)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/openapi/python_examples.py#L626'>zerver/openapi/python_examples.py:626:25:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/openapi/python_examples.py#L629'>zerver/openapi/python_examples.py:629:28:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_auth_backends.py#L3701'>zerver/tests/test_auth_backends.py:3701:29:</a> RUF015 [*] Prefer `next(iter(oidc_setting_dict.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_auth_backends.py#L3751'>zerver/tests/test_auth_backends.py:3751:27:</a> RUF015 [*] Prefer `next(iter(mock_oidc_setting_dict.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_auth_backends.py#L3766'>zerver/tests/test_auth_backends.py:3766:27:</a> RUF015 [*] Prefer `next(iter(mock_oidc_setting_dict.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_event_system.py#L588'>zerver/tests/test_event_system.py:588:30:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_event_system.py#L797'>zerver/tests/test_event_system.py:797:26:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_event_system.py#L808'>zerver/tests/test_event_system.py:808:26:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_events.py#L1083'>zerver/tests/test_events.py:1083:26:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_events.py#L1094'>zerver/tests/test_events.py:1094:26:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_gitter_importer.py#L103'>zerver/tests/test_gitter_importer.py:103:45:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_gitter_importer.py#L98'>zerver/tests/test_gitter_importer.py:98:41:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_logging_handlers.py#L69'>zerver/tests/test_logging_handlers.py:69:16:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_fetch.py#L2811'>zerver/tests/test_message_fetch.py:2811:27:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_send.py#L2001'>zerver/tests/test_message_send.py:2001:31:</a> RUF015 [*] Prefer `next(iter(recent_conversations.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_send.py#L590'>zerver/tests/test_message_send.py:590:31:</a> RUF015 [*] Prefer `next(iter(recent_conversations.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_send.py#L591'>zerver/tests/test_message_send.py:591:24:</a> RUF015 [*] Prefer `next(iter(recent_conversations.keys()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_send.py#L615'>zerver/tests/test_message_send.py:615:31:</a> RUF015 [*] Prefer `next(iter(recent_conversations.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_message_send.py#L616'>zerver/tests/test_message_send.py:616:24:</a> RUF015 [*] Prefer `next(iter(recent_conversations.keys()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/tests/test_users.py#L2161'>zerver/tests/test_users.py:2161:27:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/realm_emoji.py#L43'>zerver/views/realm_emoji.py:43:18:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/realm_icon.py#L22'>zerver/views/realm_icon.py:22:17:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/realm_logo.py#L28'>zerver/views/realm_logo.py:28:17:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/upload.py#L272'>zerver/views/upload.py:272:17:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/user_settings.py#L366'>zerver/views/user_settings.py:366:17:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/users.py#L417'>zerver/views/users.py:417:21:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/views/users.py#L559'>zerver/views/users.py:559:21:</a> RUF015 [*] Prefer `next(iter(request.FILES.values()))` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zproject/backends.py#L2847'>zproject/backends.py:2847:40:</a> RUF015 [*] Prefer `next(...)` over single element slice
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zproject/backends.py#L2874'>zproject/backends.py:2874:27:</a> RUF015 [*] Prefer `next(...)` over single element slice
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF015 | 72 | 72 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.03ms     4.2 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1899.3±2.51µs     8.8 MB/sec    1.01   1908.9±4.12µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    206.8±1.70µs    14.3 MB/sec    1.01    208.4±1.99µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.06ms     3.0 MB/sec    1.01     13.6±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.01      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.5±1.32µs     7.9 MB/sec    1.00    371.6±1.25µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.01      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.5±2.23µs    11.6 MB/sec    1.01   1448.0±1.49µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.9±0.23µs    19.7 MB/sec    1.01    151.2±0.26µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.1 MB/sec    1.01      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.26ms     3.6 MB/sec    1.03     11.5±0.45ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.7 MB/sec    1.04      2.2±0.12ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    242.2±8.51µs    12.2 MB/sec    1.01   245.2±10.61µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.16ms     5.3 MB/sec    1.01      4.9±0.18ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.43ms     2.6 MB/sec    1.00     15.9±0.48ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.09ms     4.1 MB/sec    1.04      4.2±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   505.0±25.17µs     5.8 MB/sec    1.00   505.8±22.29µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.21ms     3.6 MB/sec    1.01      7.2±0.22ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.19ms     4.9 MB/sec    1.00      8.3±0.25ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1704.3±40.36µs     9.8 MB/sec    1.02  1732.4±59.36µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.1±3.46µs    15.4 MB/sec    1.04   198.4±12.09µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.0 MB/sec    1.02      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-15 00:21_

I need to change the diagnostic to avoid including the entire snippet if it's multi-line.

---

_Comment by @charliermarsh on 2023-07-15 00:22_

It's a lot of new violations but they generally look correct to me.

---

_@dhruvmanila approved on 2023-07-15 13:18_

Nice!

---

_Comment by @hoxbro on 2023-07-15 20:52_

Sorry for the noise, but isn't it possible to reduce `next(iter(i + 1 for i in x))` down to `next(i + 1 for i in x)`?

---

_Comment by @charliermarsh on 2023-07-15 20:55_

Good point!

---

_Review requested from @zanieb by @charliermarsh on 2023-07-17 02:08_

---

_Comment by @charliermarsh on 2023-07-20 02:07_

@zanieb - Do you mind taking a look at this one? I'd love to get more sets of eyes on whether this is a safe / correct refactor from the Python perspective.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:90 on 2023-07-20 06:59_

What I understand is that this replaces the whole `iterable` with `...` . Is this the intended behavior? 

Also be aware, you compare against byte length, not character length (or width)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:238 on 2023-07-20 07:02_

Nit: I personally prefer match statements when handling both the `Some` and `None` case (It's likely that our clippy pedantic complains that you should use `if... else`. I strongly disagree with that because I find match easier to read but that works too :wink: 

```suggestion
                }) => match match_simple_comprehension(elt, generators) {
                	Some(range) =>  IterationTarget { range, iterable: false },
                	None => IterationTarget { range: arg.range(), iterable: true },
              	},
```


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:281 on 2023-07-20 07:04_

Nit: This seems to be the only branch that returns `None`. I then prefer to write:

```rust
let result = match arg {
	...
	// Returns no option
	Expr::ListComp(...) => IterationTarget {... },
	
	_ => return None;
};

Some(result)
```

It safes you from having to write that many `Some`'s 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:299 on 2023-07-20 07:06_

Nit: You can go funky on match statements

```suggestion
fn match_simple_comprehension(elt: &Expr, generators: &[Comprehension]) -> Option<TextRange> {
    let [generator @ Comprehension { is_async: false, ... } ] = generators else {
        return None;
    };
```

---

_@MichaReiser reviewed on 2023-07-20 07:07_

---

_@charliermarsh reviewed on 2023-07-20 16:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:90 on 2023-07-20 16:30_

Yes, it's intended. It avoids excessively long diagnostic messages.

---

_Comment by @zanieb on 2023-07-20 16:35_

I've given the ecosystem changes a look and thought about the rule a bit. I can't really think of any downsides here other than that it's a little advanced for most users in comparison with `list(...)[0]` and the `StopIteration` error is definitely less than ideal. I think "suggested" is a good applicability for this fix.

---

_Merged by @charliermarsh on 2023-07-21 00:08_

---

_Closed by @charliermarsh on 2023-07-21 00:08_

---

_Branch deleted on 2023-07-21 00:08_

---

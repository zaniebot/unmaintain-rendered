```yaml
number: 5508
title: Avoid PERF rules for iteration-dependent assignments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/perf
created_at: 2023-07-04T18:12:16Z
updated_at: 2023-07-04T18:43:56Z
url: https://github.com/astral-sh/ruff/pull/5508
synced_at: 2026-01-12T03:36:55Z
```

# Avoid PERF rules for iteration-dependent assignments

---

_Pull request opened by @charliermarsh on 2023-07-04 18:12_

## Summary

We need to avoid raising "rewrite as a comprehension" violations in cases like:

```python
d = defaultdict(list)

for i in [1, 2, 3]:
    d[i].append(i**2)
```

Closes https://github.com/astral-sh/ruff/issues/5494.

Closes https://github.com/astral-sh/ruff/issues/5500.


---

_Label `bug` added by @charliermarsh on 2023-07-04 18:12_

---

_Merged by @charliermarsh on 2023-07-04 18:21_

---

_Closed by @charliermarsh on 2023-07-04 18:21_

---

_Branch deleted on 2023-07-04 18:21_

---

_Comment by @github-actions[bot] on 2023-07-04 18:24_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -55, 0 error(s))

<details><summary>airflow (+0, -25)</summary>
<p>

```diff
- airflow/cli/commands/connection_command.py:147:13: PERF401 Use a list comprehension to create a transformed list
- airflow/cli/commands/dag_command.py:380:9: PERF401 Use a list comprehension to create a transformed list
- airflow/cli/commands/role_command.py:50:13: PERF401 Use a list comprehension to create a transformed list
- airflow/cli/commands/user_command.py:220:21: PERF401 Use a list comprehension to create a transformed list
- airflow/dag_processing/manager.py:865:13: PERF401 Use a list comprehension to create a transformed list
- airflow/kubernetes/pod_generator_deprecated.py:174:21: PERF401 Use a list comprehension to create a transformed list
- airflow/migrations/utils.py:49:9: PERF401 Use a list comprehension to create a transformed list
- airflow/models/taskinstance.py:2148:17: PERF402 Use `list` or `list.copy` to create a copy of a list
- airflow/models/taskinstance.py:2647:25: PERF401 Use a list comprehension to create a transformed list
- airflow/models/taskinstance.py:2657:25: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/cncf/kubernetes/backcompat/backwards_compat_converters.py:77:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/exasol/hooks/exasol.py:147:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:255:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:258:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:234:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:322:13: PERF401 Use a list comprehension to create a transformed list
- airflow/providers/google/cloud/transfers/sql_to_gcs.py:226:17: PERF401 Use a list comprehension to create a transformed list
- airflow/utils/entry_points.py:40:17: PERF401 Use a list comprehension to create a transformed list
- airflow/www/views.py:445:17: PERF402 Use `list` or `list.copy` to create a copy of a list
- dev/breeze/src/airflow_breeze/utils/parallel.py:247:17: PERF401 Use a list comprehension to create a transformed list
- tests/models/test_dag.py:895:13: PERF401 Use a list comprehension to create a transformed list
- tests/providers/amazon/aws/transfers/test_sql_to_s3.py:208:13: PERF401 Use a list comprehension to create a transformed list
- tests/providers/amazon/aws/transfers/test_sql_to_s3.py:259:13: PERF401 Use a list comprehension to create a transformed list
- tests/providers/google/cloud/hooks/test_pubsub.py:395:13: PERF401 Use a list comprehension to create a transformed list
- tests/utils/test_log_handlers.py:562:9: PERF401 Use a list comprehension to create a transformed list
```

</p>
</details>
<details><summary>bokeh (+0, -6)</summary>
<p>

```diff
- examples/topics/pie/donut.py:53:5: PERF401 Use a list comprehension to create a transformed list
- src/bokeh/core/has_props.py:806:9: PERF401 Use a list comprehension to create a transformed list
- src/bokeh/embed/util.py:334:9: PERF401 Use a list comprehension to create a transformed list
- src/bokeh/models/renderers/glyph_renderer.py:166:17: PERF401 Use a list comprehension to create a transformed list
- src/bokeh/sphinxext/bokeh_sampledata_xref.py:202:17: PERF401 Use a list comprehension to create a transformed list
- tests/codebase/test_no_request_host.py:48:17: PERF401 Use a list comprehension to create a transformed list
```

</p>
</details>
<details><summary>zulip (+0, -24)</summary>
<p>

```diff
- scripts/lib/check_rabbitmq_queue.py:173:9: PERF401 Use a list comprehension to create a transformed list
- tools/lib/html_branches.py:97:17: PERF401 Use a list comprehension to create a transformed list
- zerver/actions/message_flags.py:239:9: PERF401 Use a list comprehension to create a transformed list
- zerver/actions/streams.py:234:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/actions/streams.py:501:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/actions/users.py:457:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/data_import/rocketchat.py:281:9: PERF401 Use a list comprehension to create a transformed list
- zerver/data_import/slack.py:806:17: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/generate_test_data.py:33:9: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/home.py:58:9: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/message.py:1598:9: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/message.py:263:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/lib/soft_deactivation.py:194:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/lib/soft_deactivation.py:230:9: PERF402 Use `list` or `list.copy` to create a copy of a list
- zerver/lib/topic.py:216:9: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/user_groups.py:118:9: PERF401 Use a list comprehension to create a transformed list
- zerver/lib/user_groups.py:124:9: PERF401 Use a list comprehension to create a transformed list
- zerver/migrations/0436_realmauthenticationmethods.py:18:17: PERF401 Use a list comprehension to create a transformed list
- zerver/openapi/markdown_extension.py:347:13: PERF401 Use a list comprehension to create a transformed list
- zerver/tests/test_message_fetch.py:2712:13: PERF401 Use a list comprehension to create a transformed list
- zerver/tests/test_migrations.py:70:13: PERF401 Use a list comprehension to create a transformed list
- zerver/views/streams.py:523:9: PERF401 Use a list comprehension to create a transformed list
- zerver/views/streams.py:525:9: PERF401 Use a list comprehension to create a transformed list
- zilencer/management/commands/populate_db.py:1179:9: PERF401 Use a list comprehension to create a transformed list
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.3±0.02ms     4.9 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1783.9±3.51µs     9.3 MB/sec    1.00   1768.9±5.42µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    196.5±1.00µs    15.0 MB/sec    1.00    197.4±3.91µs    14.9 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.03ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.06ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.3±1.55µs     8.1 MB/sec    1.00    365.4±1.56µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.3±0.28ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.6±3.41µs    11.2 MB/sec    1.00   1486.1±1.97µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.6±0.43µs    18.6 MB/sec    1.01    159.6±4.54µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.04ms     4.3 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.02ms     8.3 MB/sec    1.00  1988.9±22.21µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    217.8±2.61µs    13.5 MB/sec    1.01   220.6±12.04µs    13.4 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.03ms     5.7 MB/sec    1.00      4.5±0.04ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.6±0.12ms     2.6 MB/sec    1.00     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.2±6.51µs     6.8 MB/sec    1.00    432.1±6.81µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1668.1±12.51µs    10.0 MB/sec    1.00  1669.0±10.28µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.2±1.77µs    16.3 MB/sec    1.00    182.1±4.02µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

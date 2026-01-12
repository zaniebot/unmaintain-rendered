```yaml
number: 5995
title: "Extend SIM118 with `not in`"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-07-23T00:48:26Z
updated_at: 2023-07-23T02:10:52Z
url: https://github.com/astral-sh/ruff/pull/5995
synced_at: 2026-01-12T15:55:20Z
```

# Extend SIM118 with `not in`

---

_@sbrugman_

Closes https://github.com/astral-sh/ruff/issues/5989

Tracking issue https://github.com/astral-sh/ruff/issues/1348

---

_Comment by @github-actions[bot] on 2023-07-23 01:10_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+16, -0, 0 error(s))

<details><summary>airflow (+8, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/providers/amazon/aws/hooks/redshift_data.py#L188'>airflow/providers/amazon/aws/hooks/redshift_data.py:188:16:</a> SIM118 [*] Use `"NextToken" not in response` instead of `"NextToken" not in response.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/providers/apache/hive/hooks/hive.py#L710'>airflow/providers/apache/hive/hooks/hive.py:710:12:</a> SIM118 [*] Use `partition_key not in part_specs[0]` instead of `partition_key not in part_specs[0].keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/providers/google/cloud/operators/functions.py#L215'>airflow/providers/google/cloud/operators/functions.py:215:12:</a> SIM118 [*] Use `"labels" not in self.body` instead of `"labels" not in self.body.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/www/extensions/init_appbuilder.py#L257'>airflow/www/extensions/init_appbuilder.py:257:16:</a> SIM118 [*] Use `baseview.__class__.__name__ not in self.get_app.blueprints` instead of `baseview.__class__.__name__ not in self.get_app.blueprints.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/helm_tests/other/test_keda.py#L194'>helm_tests/other/test_keda.py:194:16:</a> SIM118 [*] Use `"kedaConnection" not in secret_data` instead of `"kedaConnection" not in secret_data.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/helm_tests/other/test_keda.py#L230'>helm_tests/other/test_keda.py:230:16:</a> SIM118 [*] Use `"kedaConnection" not in secret_data` instead of `"kedaConnection" not in secret_data.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/helm_tests/other/test_redis.py#L76'>helm_tests/other/test_redis.py:76:20:</a> SIM118 [*] Use `REDIS_OBJECTS["SECRET_PASSWORD"] not in k8s_obj_by_key` instead of `REDIS_OBJECTS["SECRET_PASSWORD"] not in k8s_obj_by_key.keys()`
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/helm_tests/other/test_redis.py#L83'>helm_tests/other/test_redis.py:83:20:</a> SIM118 [*] Use `REDIS_OBJECTS["SECRET_BROKER_URL"] not in k8s_obj_by_key` instead of `REDIS_OBJECTS["SECRET_BROKER_URL"] not in k8s_obj_by_key.keys()`
</pre>

</p>
</details>
<details><summary>zulip (+8, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/event_schema.py#L1477'>zerver/lib/event_schema.py:1477:16:</a> SIM118 [*] Use `"language_name" not in event` instead of `"language_name" not in event.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/event_schema.py#L1498'>zerver/lib/event_schema.py:1498:16:</a> SIM118 [*] Use `"language_name" not in event` instead of `"language_name" not in event.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/event_schema.py#L878'>zerver/lib/event_schema.py:878:12:</a> SIM118 [*] Use `"extra_data" not in event` instead of `"extra_data" not in event.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/external_accounts.py#L68'>zerver/lib/external_accounts.py:68:8:</a> SIM118 [*] Use `field_subtype not in DEFAULT_EXTERNAL_ACCOUNTS` instead of `field_subtype not in DEFAULT_EXTERNAL_ACCOUNTS.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/external_accounts.py#L70'>zerver/lib/external_accounts.py:70:16:</a> SIM118 [*] Use `"url_pattern" not in field_data` instead of `"url_pattern" not in field_data.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/lib/markdown/__init__.py#L1150'>zerver/lib/markdown/__init__.py:1150:20:</a> SIM118 [*] Use `"class" not in uncle` instead of `"class" not in uncle.keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/openapi/openapi.py#L378'>zerver/openapi/openapi.py:378:8:</a> SIM118 [*] Use `path not in openapi_spec.openapi()["paths"]` instead of `path not in openapi_spec.openapi()["paths"].keys()`
+ <a href='https://github.com/zulip/zulip/blob/7746e11486e283af6432914505ec789c5204e733/zerver/webhooks/opbeat/view.py#L65'>zerver/webhooks/opbeat/view.py:65:8:</a> SIM118 [*] Use `subject_type not in subject_types` instead of `subject_type not in subject_types.keys()`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM118 | 16 | 16 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.06ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1854.4±1.75µs     9.0 MB/sec    1.00   1846.6±2.15µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    202.7±0.67µs    14.6 MB/sec    1.00    202.9±0.26µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.07ms     3.1 MB/sec    1.00     12.9±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.1±1.65µs     6.9 MB/sec    1.01    432.2±0.47µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.01ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1405.0±2.96µs    11.9 MB/sec    1.00   1407.3±4.19µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.9±1.51µs    19.0 MB/sec    1.00    155.0±0.24µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.3±0.11ms     3.6 MB/sec    1.00     11.1±0.14ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.8±5.56µs    12.2 MB/sec    1.00    243.7±5.89µs    12.1 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.07ms     5.3 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.18ms     2.6 MB/sec    1.00     15.6±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   498.5±15.06µs     5.9 MB/sec    1.00   484.1±11.96µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.11ms     3.5 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.07ms     4.9 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1713.7±19.46µs     9.7 MB/sec    1.00  1687.0±28.89µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±3.19µs    15.4 MB/sec    1.00    190.7±5.91µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.04ms     6.9 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-23 01:37_

---

_Merged by @charliermarsh on 2023-07-23 01:46_

---

_Closed by @charliermarsh on 2023-07-23 01:46_

---

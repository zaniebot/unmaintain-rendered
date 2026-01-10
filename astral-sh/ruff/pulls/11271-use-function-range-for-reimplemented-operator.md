```yaml
number: 11271
title: "Use function range for `reimplemented-operator` diagnostics"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/ident
created_at: 2024-05-03T20:04:51Z
updated_at: 2024-05-03T20:17:51Z
url: https://github.com/astral-sh/ruff/pull/11271
synced_at: 2026-01-10T22:37:02Z
```

# Use function range for `reimplemented-operator` diagnostics

---

_Pull request opened by @charliermarsh on 2024-05-03 20:04_

_No description provided._

---

_Label `rule` added by @charliermarsh on 2024-05-03 20:05_

---

_Label `preview` added by @charliermarsh on 2024-05-03 20:05_

---

_Merged by @charliermarsh on 2024-05-03 20:11_

---

_Closed by @charliermarsh on 2024-05-03 20:11_

---

_Branch deleted on 2024-05-03 20:11_

---

_Comment by @github-actions[bot] on 2024-05-03 20:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -18 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+13 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/api_internal/endpoints/test_rpc_api_endpoint.py#L65'>tests/api_internal/endpoints/test_rpc_api_endpoint.py:65:1:</a> FURB118 Use `operator.eq` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/api_internal/endpoints/test_rpc_api_endpoint.py#L65'>tests/api_internal/endpoints/test_rpc_api_endpoint.py:65:5:</a> FURB118 Use `operator.eq` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/decorators/test_python.py#L701'>tests/decorators/test_python.py:701:5:</a> FURB118 Use `operator.mul` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/decorators/test_python.py#L702'>tests/decorators/test_python.py:702:9:</a> FURB118 Use `operator.mul` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/models/test_baseoperator.py#L962'>tests/models/test_baseoperator.py:962:5:</a> FURB118 Use `operator.add` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/models/test_baseoperator.py#L963'>tests/models/test_baseoperator.py:963:9:</a> FURB118 Use `operator.add` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/operators/test_python.py#L1487'>tests/operators/test_python.py:1487:13:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/operators/test_python.py#L1487'>tests/operators/test_python.py:1487:9:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/operators/test_python.py#L812'>tests/operators/test_python.py:812:13:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/operators/test_python.py#L812'>tests/operators/test_python.py:812:9:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L102'>tests/providers/amazon/aws/hooks/test_rds.py:102:1:</a> FURB118 Use `operator.itemgetter("DBClusterSnapshotIdentifier")` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L103'>tests/providers/amazon/aws/hooks/test_rds.py:103:5:</a> FURB118 Use `operator.itemgetter("DBClusterSnapshotIdentifier")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L80'>tests/providers/amazon/aws/hooks/test_rds.py:80:1:</a> FURB118 Use `operator.itemgetter("DBSnapshotIdentifier")` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L81'>tests/providers/amazon/aws/hooks/test_rds.py:81:5:</a> FURB118 Use `operator.itemgetter("DBSnapshotIdentifier")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L85'>tests/providers/amazon/aws/hooks/test_rds.py:85:1:</a> FURB118 Use `operator.itemgetter("DBSnapshotArn")` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/providers/amazon/aws/hooks/test_rds.py#L86'>tests/providers/amazon/aws/hooks/test_rds.py:86:5:</a> FURB118 Use `operator.itemgetter("DBSnapshotArn")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/serialization/test_serialized_objects.py#L145'>tests/serialization/test_serialized_objects.py:145:1:</a> FURB118 Use `operator.eq` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/serialization/test_serialized_objects.py#L145'>tests/serialization/test_serialized_objects.py:145:5:</a> FURB118 Use `operator.eq` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/system/providers/amazon/aws/example_ec2.py#L90'>tests/system/providers/amazon/aws/example_ec2.py:90:1:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/system/providers/amazon/aws/example_ec2.py#L91'>tests/system/providers/amazon/aws/example_ec2.py:91:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/system/providers/amazon/aws/example_emr.py#L111'>tests/system/providers/amazon/aws/example_emr.py:111:1:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/system/providers/amazon/aws/example_emr.py#L112'>tests/system/providers/amazon/aws/example_emr.py:112:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L111'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:111:9:</a> FURB118 Use `operator.add` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L112'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:112:13:</a> FURB118 Use `operator.add` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L197'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:197:9:</a> FURB118 Use `operator.add` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/795592c8baf9ae732563e6958c3a0ad3a168c3f6/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L198'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:198:13:</a> FURB118 Use `operator.add` instead of defining a function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/tools/setup/emoji/emoji_setup_utils.py#L105'>tools/setup/emoji/emoji_setup_utils.py:105:1:</a> FURB118 Use `operator.itemgetter("has_img_google")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/tools/setup/emoji/emoji_setup_utils.py#L105'>tools/setup/emoji/emoji_setup_utils.py:105:5:</a> FURB118 Use `operator.itemgetter("has_img_google")` instead of defining a function
- <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L109'>zerver/lib/display_recipient.py:109:5:</a> FURB118 Use `operator.itemgetter("recipient_id")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L109'>zerver/lib/display_recipient.py:109:9:</a> FURB118 Use `operator.itemgetter("recipient_id")` instead of defining a function
- <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L112'>zerver/lib/display_recipient.py:112:5:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L112'>zerver/lib/display_recipient.py:112:9:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a function
- <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L67'>zerver/lib/display_recipient.py:67:1:</a> FURB118 Use `operator.itemgetter("id")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/display_recipient.py#L67'>zerver/lib/display_recipient.py:67:5:</a> FURB118 Use `operator.itemgetter("id")` instead of defining a function
- <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/webhooks/git.py#L405'>zerver/lib/webhooks/git.py:405:1:</a> FURB118 Use `operator.itemgetter(slice(11))` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/5acd059c3891ddfb5f1f7397e3c190122759112a/zerver/lib/webhooks/git.py#L405'>zerver/lib/webhooks/git.py:405:5:</a> FURB118 Use `operator.itemgetter(slice(11))` instead of defining a function
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 36 | 18 | 18 | 0 | 0 |

</p>
</details>




---

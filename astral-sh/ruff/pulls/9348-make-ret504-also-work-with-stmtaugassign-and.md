```yaml
number: 9348
title: Make RET504 also work with StmtAugAssign and StmtAnnAssign AST nodes
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
base: main
head: evanrittenhouse/9327
created_at: 2024-01-01T16:06:03Z
updated_at: 2024-04-08T06:59:50Z
url: https://github.com/astral-sh/ruff/pull/9348
synced_at: 2026-01-12T15:55:28Z
```

# Make RET504 also work with StmtAugAssign and StmtAnnAssign AST nodes

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Closes #9327 

## Test Plan

<!-- How was it tested? -->
Unit tests

---

_Comment by @Skylion007 on 2024-01-01 17:27_

This feels like it should be controlled by a rule level setting as this return statement has slightly different semantics if it's a list / mutable type. Perhaps the additional constraint that this only be applied if the object is an immutable type should be applied?

```python
a = [1]
def f(c):
    c += [2]
    return c

assert a is f(a)

c = [1]

def g(c):
    return c + [2]

assert c is g(c)
```
the top assertion will pass while the 2nd assertion will fail.

---

_Comment by @evanrittenhouse on 2024-01-01 17:39_

> Perhaps the additional constraint that this only be applied if the object is an immutable type should be applied?

AFAIK Ruff doesn't know how to check static types (maybe something's changed since I was last around). I'm happy to remove the `Name` clause in the match statement and leave it as only making the change if we see a `NumberLiteral` though

---

_Comment by @github-actions[bot] on 2024-01-01 17:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+58 -0 violations, +0 -0 fixes in 5 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/example_dags/example_bash_decorator.py#L106'>airflow/example_dags/example_bash_decorator.py:106:16:</a> RET504 Unnecessary assignment to `cmd` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L193'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:193:20:</a> RET504 Unnecessary assignment to `snapshot_status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L570'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:570:16:</a> RET504 Unnecessary assignment to `pod` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/cncf/kubernetes/operators/pod.py#L800'>airflow/providers/cncf/kubernetes/operators/pod.py:800:16:</a> RET504 Unnecessary assignment to `labels_value` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/common/sql/hooks/sql.py#L511'>airflow/providers/common/sql/hooks/sql.py:511:16:</a> RET504 Unnecessary assignment to `sql` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/dbt/cloud/hooks/dbt.py#L260'>airflow/providers/dbt/cloud/hooks/dbt.py:260:16:</a> RET504 Unnecessary assignment to `job_run_status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/hooks/dataflow.py#L1355'>airflow/providers/google/cloud/hooks/dataflow.py:1355:16:</a> RET504 Unnecessary assignment to `page_result` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/hooks/kubernetes_engine.py#L490'>airflow/providers/google/cloud/hooks/kubernetes_engine.py:490:20:</a> RET504 Unnecessary assignment to `pod` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/operators/bigquery.py#L1001'>airflow/providers/google/cloud/operators/bigquery.py:1001:16:</a> RET504 Unnecessary assignment to `query` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/operators/mlengine.py#L78'>airflow/providers/google/cloud/operators/mlengine.py:78:12:</a> RET504 Unnecessary assignment to `cleansed_job_id` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/microsoft/azure/hooks/data_factory.py#L1204'>airflow/providers/microsoft/azure/hooks/data_factory.py:1204:16:</a> RET504 Unnecessary assignment to `status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/openai/hooks/openai.py#L93'>airflow/providers/openai/hooks/openai.py:93:16:</a> RET504 Unnecessary assignment to `embeddings` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/weaviate/hooks/weaviate.py#L514'>airflow/providers/weaviate/hooks/weaviate.py:514:16:</a> RET504 Unnecessary assignment to `results` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/weaviate/hooks/weaviate.py#L533'>airflow/providers/weaviate/hooks/weaviate.py:533:16:</a> RET504 Unnecessary assignment to `results` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/serialization/serde.py#L313'>airflow/serialization/serde.py:313:12:</a> RET504 Unnecessary assignment to `s` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/assign_cherry_picked_prs_with_milestone.py#L168'>dev/assign_cherry_picked_prs_with_milestone.py:168:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L127'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:127:12:</a> RET504 Unnecessary assignment to `port_map` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/packages.py#L599'>dev/breeze/src/airflow_breeze/utils/packages.py:599:12:</a> RET504 Unnecessary assignment to `context` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/packages.py#L631'>dev/breeze/src/airflow_breeze/utils/packages.py:631:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/parallel.py#L193'>dev/breeze/src/airflow_breeze/utils/parallel.py:193:24:</a> RET504 Unnecessary assignment to `list_to_return` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/run_utils.py#L195'>dev/breeze/src/airflow_breeze/utils/run_utils.py:195:12:</a> RET504 Unnecessary assignment to `env_to_print` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_bulk_issues.py#L105'>dev/prepare_bulk_issues.py:105:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_bulk_issues.py#L79'>dev/prepare_bulk_issues.py:79:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_release_issue.py#L200'>dev/prepare_release_issue.py:200:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/io/webdriver.py#L129'>src/bokeh/io/webdriver.py:129:12:</a> RET504 Unnecessary assignment to `device_pixel_ratio` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/model/docs.py#L86'>src/bokeh/model/docs.py:86:12:</a> RET504 Unnecessary assignment to `html` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/protocol/message.py#L327'>src/bokeh/protocol/message.py:327:20:</a> RET504 Unnecessary assignment to `sent` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L219'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:219:12:</a> RET504 Unnecessary assignment to `line` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_parsers.py#L155'>securedrop/pretty_bad_protocol/_parsers.py:155:12:</a> RET504 Unnecessary assignment to `ret` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/38421ab55c51f989afe98ebd0276528833e6f868/src/scikit_build_core/build/_editable.py#L52'>src/scikit_build_core/build/_editable.py:52:12:</a> RET504 Unnecessary assignment to `editable_txt` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/tools/lib/provision_inner.py#L78'>tools/lib/provision_inner.py:78:12:</a> RET504 Unnecessary assignment to `paths` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/tools/lib/provision_inner.py#L85'>tools/lib/provision_inner.py:85:12:</a> RET504 Unnecessary assignment to `paths` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/tools/lib/test_script.py#L31'>tools/lib/test_script.py:31:12:</a> RET504 Unnecessary assignment to `text` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/actions/invites.py#L423'>zerver/actions/invites.py:423:12:</a> RET504 Unnecessary assignment to `confirmations` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/ccache.py#L197'>zerver/lib/ccache.py:197:12:</a> RET504 Unnecessary assignment to `out` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/ccache.py#L214'>zerver/lib/ccache.py:214:12:</a> RET504 Unnecessary assignment to `out` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/display_recipient.py#L126'>zerver/lib/display_recipient.py:126:12:</a> RET504 Unnecessary assignment to `stream_display_recipients` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/message.py#L1365'>zerver/lib/message.py:1365:12:</a> RET504 Unnecessary assignment to `result` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/scheduled_messages.py#L36'>zerver/lib/scheduled_messages.py:36:12:</a> RET504 Unnecessary assignment to `scheduled_message_dicts` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/migrations/0383_revoke_invitations_from_deactivated_users.py#L45'>zerver/migrations/0383_revoke_invitations_from_deactivated_users.py:45:16:</a> RET504 Unnecessary assignment to `confirmation_ids` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/tests/test_auth_backends.py#L2076'>zerver/tests/test_auth_backends.py:2076:16:</a> RET504 Unnecessary assignment to `saml_response` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/tests/test_auth_backends.py#L2089'>zerver/tests/test_auth_backends.py:2089:16:</a> RET504 Unnecessary assignment to `logout_request` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/views/message_fetch.py#L66'>zerver/views/message_fetch.py:66:12:</a> RET504 Unnecessary assignment to `result` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/github/view.py#L344'>zerver/webhooks/github/view.py:344:12:</a> RET504 Unnecessary assignment to `action` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/librato/view.py#L131'>zerver/webhooks/librato/view.py:131:16:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/librato/view.py#L158'>zerver/webhooks/librato/view.py:158:16:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/linear/view.py#L55'>zerver/webhooks/linear/view.py:55:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/linear/view.py#L75'>zerver/webhooks/linear/view.py:75:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/raygun/view.py#L144'>zerver/webhooks/raygun/view.py:144:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/raygun/view.py#L202'>zerver/webhooks/raygun/view.py:202:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET504 | 58 | 58 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+54 -0 violations, +0 -0 fixes in 5 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+26 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/example_dags/example_bash_decorator.py#L106'>airflow/example_dags/example_bash_decorator.py:106:16:</a> RET504 Unnecessary assignment to `cmd` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L193'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:193:20:</a> RET504 Unnecessary assignment to `snapshot_status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L570'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:570:16:</a> RET504 Unnecessary assignment to `pod` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/cncf/kubernetes/operators/pod.py#L800'>airflow/providers/cncf/kubernetes/operators/pod.py:800:16:</a> RET504 Unnecessary assignment to `labels_value` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/common/sql/hooks/sql.py#L511'>airflow/providers/common/sql/hooks/sql.py:511:16:</a> RET504 Unnecessary assignment to `sql` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/dbt/cloud/hooks/dbt.py#L260'>airflow/providers/dbt/cloud/hooks/dbt.py:260:16:</a> RET504 Unnecessary assignment to `job_run_status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/hooks/dataflow.py#L1355'>airflow/providers/google/cloud/hooks/dataflow.py:1355:16:</a> RET504 Unnecessary assignment to `page_result` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/hooks/kubernetes_engine.py#L490'>airflow/providers/google/cloud/hooks/kubernetes_engine.py:490:20:</a> RET504 Unnecessary assignment to `pod` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/operators/mlengine.py#L78'>airflow/providers/google/cloud/operators/mlengine.py:78:12:</a> RET504 Unnecessary assignment to `cleansed_job_id` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/microsoft/azure/hooks/data_factory.py#L1204'>airflow/providers/microsoft/azure/hooks/data_factory.py:1204:16:</a> RET504 Unnecessary assignment to `status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/openai/hooks/openai.py#L93'>airflow/providers/openai/hooks/openai.py:93:16:</a> RET504 Unnecessary assignment to `embeddings` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/weaviate/hooks/weaviate.py#L514'>airflow/providers/weaviate/hooks/weaviate.py:514:16:</a> RET504 Unnecessary assignment to `results` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/weaviate/hooks/weaviate.py#L533'>airflow/providers/weaviate/hooks/weaviate.py:533:16:</a> RET504 Unnecessary assignment to `results` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/serialization/serde.py#L313'>airflow/serialization/serde.py:313:12:</a> RET504 Unnecessary assignment to `s` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/assign_cherry_picked_prs_with_milestone.py#L168'>dev/assign_cherry_picked_prs_with_milestone.py:168:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L127'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:127:12:</a> RET504 Unnecessary assignment to `port_map` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/packages.py#L599'>dev/breeze/src/airflow_breeze/utils/packages.py:599:12:</a> RET504 Unnecessary assignment to `context` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/packages.py#L631'>dev/breeze/src/airflow_breeze/utils/packages.py:631:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/breeze/src/airflow_breeze/utils/parallel.py#L193'>dev/breeze/src/airflow_breeze/utils/parallel.py:193:24:</a> RET504 Unnecessary assignment to `list_to_return` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_bulk_issues.py#L105'>dev/prepare_bulk_issues.py:105:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_bulk_issues.py#L79'>dev/prepare_bulk_issues.py:79:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/dev/prepare_release_issue.py#L200'>dev/prepare_release_issue.py:200:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py#L124'>scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:124:12:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/scripts/in_container/run_provider_yaml_files_check.py#L133'>scripts/in_container/run_provider_yaml_files_check.py:133:12:</a> RET504 Unnecessary assignment to `all_integrations` before `return` statement
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/io/webdriver.py#L129'>src/bokeh/io/webdriver.py:129:12:</a> RET504 Unnecessary assignment to `device_pixel_ratio` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/model/docs.py#L86'>src/bokeh/model/docs.py:86:12:</a> RET504 Unnecessary assignment to `html` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/protocol/message.py#L327'>src/bokeh/protocol/message.py:327:20:</a> RET504 Unnecessary assignment to `sent` before `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L219'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:219:12:</a> RET504 Unnecessary assignment to `line` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_parsers.py#L155'>securedrop/pretty_bad_protocol/_parsers.py:155:12:</a> RET504 Unnecessary assignment to `ret` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/38421ab55c51f989afe98ebd0276528833e6f868/src/scikit_build_core/build/_editable.py#L52'>src/scikit_build_core/build/_editable.py:52:12:</a> RET504 Unnecessary assignment to `editable_txt` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+22 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/tools/lib/test_script.py#L31'>tools/lib/test_script.py:31:12:</a> RET504 Unnecessary assignment to `text` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/actions/invites.py#L423'>zerver/actions/invites.py:423:12:</a> RET504 Unnecessary assignment to `confirmations` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/ccache.py#L197'>zerver/lib/ccache.py:197:12:</a> RET504 Unnecessary assignment to `out` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/ccache.py#L214'>zerver/lib/ccache.py:214:12:</a> RET504 Unnecessary assignment to `out` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/display_recipient.py#L126'>zerver/lib/display_recipient.py:126:12:</a> RET504 Unnecessary assignment to `stream_display_recipients` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/message.py#L1365'>zerver/lib/message.py:1365:12:</a> RET504 Unnecessary assignment to `result` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/lib/scheduled_messages.py#L36'>zerver/lib/scheduled_messages.py:36:12:</a> RET504 Unnecessary assignment to `scheduled_message_dicts` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/migrations/0383_revoke_invitations_from_deactivated_users.py#L45'>zerver/migrations/0383_revoke_invitations_from_deactivated_users.py:45:16:</a> RET504 Unnecessary assignment to `confirmation_ids` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/tests/test_auth_backends.py#L2076'>zerver/tests/test_auth_backends.py:2076:16:</a> RET504 Unnecessary assignment to `saml_response` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/tests/test_auth_backends.py#L2089'>zerver/tests/test_auth_backends.py:2089:16:</a> RET504 Unnecessary assignment to `logout_request` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/views/message_fetch.py#L66'>zerver/views/message_fetch.py:66:12:</a> RET504 Unnecessary assignment to `result` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/github/view.py#L344'>zerver/webhooks/github/view.py:344:12:</a> RET504 Unnecessary assignment to `action` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/librato/view.py#L131'>zerver/webhooks/librato/view.py:131:16:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/librato/view.py#L158'>zerver/webhooks/librato/view.py:158:16:</a> RET504 Unnecessary assignment to `content` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/linear/view.py#L55'>zerver/webhooks/linear/view.py:55:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/linear/view.py#L75'>zerver/webhooks/linear/view.py:75:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/raygun/view.py#L144'>zerver/webhooks/raygun/view.py:144:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/raygun/view.py#L202'>zerver/webhooks/raygun/view.py:202:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/raygun/view.py#L261'>zerver/webhooks/raygun/view.py:261:12:</a> RET504 Unnecessary assignment to `message` before `return` statement
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/webhooks/sonarqube/view.py#L108'>zerver/webhooks/sonarqube/view.py:108:12:</a> RET504 Unnecessary assignment to `msg` before `return` statement
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET504 | 54 | 54 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Skylion007 on 2024-01-01 17:58_

@evanrittenhouse I am pretty sure RUFF has very limited static type checking (basically only if the variable is initialized inside of the function). We could use that to only flag ints, floats, and other builtin immutable types.

---

_Comment by @evanrittenhouse on 2024-01-01 19:34_

We may just want to leave it as `NumberLiteral`s/string literals only, actually - we seemingly can't tell, based on the AST only, if a `ExprName` refers to something immutable or not. There's the `is_mutable_expr` helper function which we can use to check, but again, `ExprName` would be considered "immutable" there, and something like:
```python
b = []
a = [1]
b += a
return b
```
would succeed and still be "unsafe".

So the best way forward may just be to only call this on strings/numbers. Thoughts @zanieb ?

---

_Review requested from @zanieb by @zanieb on 2024-01-07 17:48_

---

_Comment by @jamesbraza on 2024-01-29 23:38_

Cc @zanieb @charliermarsh wdyt of adding this as a 0.2.0 milestone? It seems to be pretty close

---

_Comment by @zanieb on 2024-02-02 14:59_

Sorry about the delay here! I've somehow gotten to the point where GitHub mentions get buried :'( 

I think the first thing to do is to gate the behavior with preview mode — then we can stabilize the behavior later once we're confident it's correct.

We can also consider the fix safe or unsafe depending on our ability to determine the type? I think applying this fix to the following makes sense for:

- A binding that is local to the function with any type 
- An external binding with a known immutable type

For example, in the ecosystem checks we have [this violation](https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/tools/lib/provision_inner.py#L75-L78) where a mutable local variable has an augmented assignment. I guess looking at that code, I'm wondering if this change would actually be more readable? I clicked on [another random violation](https://github.com/apache/airflow/blob/9f90a655e884be0a99ee80564c41190260fa9ba1/airflow/providers/google/cloud/operators/mlengine.py#L69-L78) and don't see what value it would provide to collapse onto the `return` statement.

What do you think? @charliermarsh might have more concrete advice

---

_Comment by @MichaReiser on 2024-04-08 06:59_

I'll close this PR because it has gone stale. Feel free to reopen it if you're interested in moving the PR forward.

---

_Closed by @MichaReiser on 2024-04-08 06:59_

---

```yaml
number: 9036
title: Ignore underscore references in type annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/slf
created_at: 2023-12-07T02:48:54Z
updated_at: 2023-12-07T03:06:17Z
url: https://github.com/astral-sh/ruff/pull/9036
synced_at: 2026-01-12T15:55:27Z
```

# Ignore underscore references in type annotations

---

_@charliermarsh_

## Summary

Occasionally, valid code needs to use `argparse._SubParsersAction` in a type annotation. This isn't great, but it's indicative of the fact that public interfaces can return private types. If you accessed that private type via a private interface, then we should be flagging the call site, rather than the annotation.

Closes https://github.com/astral-sh/ruff/issues/9013.


---

_Comment by @github-actions[bot] on 2023-12-07 03:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -8 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/cli/cli_parser.py#L159'>airflow/cli/cli_parser.py:159:30:</a> SLF001 Private member accessed: `_SubParsersAction`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/cncf/kubernetes/utils/k8s_hashlib_wrapper.py#L32'>airflow/providers/cncf/kubernetes/utils/k8s_hashlib_wrapper.py:32:44:</a> SLF001 Private member accessed: `_Hash`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py#L151'>airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:151:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py#L73'>airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:73:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py#L154'>airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:154:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/utils/hashlib_wrapper.py#L29'>airflow/utils/hashlib_wrapper.py:29:44:</a> SLF001 Private member accessed: `_Hash`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zerver/lib/queue.py#L84'>zerver/lib/queue.py:84:18:</a> SLF001 Private member accessed: `_DEFAULT`
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zerver/lib/request.py#L295'>zerver/lib/request.py:295:20:</a> SLF001 Private member accessed: `_NotSpecified`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 8 | 0 | 8 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -8 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/cli/cli_parser.py#L159'>airflow/cli/cli_parser.py:159:30:</a> SLF001 Private member accessed: `_SubParsersAction`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/cncf/kubernetes/utils/k8s_hashlib_wrapper.py#L32'>airflow/providers/cncf/kubernetes/utils/k8s_hashlib_wrapper.py:32:44:</a> SLF001 Private member accessed: `_Hash`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py#L151'>airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:151:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py#L73'>airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:73:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py#L154'>airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:154:35:</a> SLF001 Private member accessed: `_ParameterSpec`
- <a href='https://github.com/apache/airflow/blob/ca20f07a16934d93792773d788b64652009065ce/airflow/utils/hashlib_wrapper.py#L29'>airflow/utils/hashlib_wrapper.py:29:44:</a> SLF001 Private member accessed: `_Hash`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zerver/lib/queue.py#L84'>zerver/lib/queue.py:84:18:</a> SLF001 Private member accessed: `_DEFAULT`
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zerver/lib/request.py#L295'>zerver/lib/request.py:295:20:</a> SLF001 Private member accessed: `_NotSpecified`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 8 | 0 | 8 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @charliermarsh on 2023-12-07 03:05_

---

_Merged by @charliermarsh on 2023-12-07 03:05_

---

_Closed by @charliermarsh on 2023-12-07 03:05_

---

_Branch deleted on 2023-12-07 03:05_

---

_Comment by @charliermarsh on 2023-12-07 03:06_

I like what I see in the ecosystem changes -- it's having the intended effect, and those are more cases in which the underscore access is valid.

---

_Label `rule` added by @charliermarsh on 2023-12-07 03:06_

---

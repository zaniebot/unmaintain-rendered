```yaml
number: 14122
title: "[`flake8-bugbear`]  Handle shadowing and scope in `unused-loop-control-variable (B007)`"
type: pull_request
state: closed
author: dylwil3
labels: []
assignees: []
draft: true
base: main
head: scope-unused
created_at: 2024-11-06T04:09:15Z
updated_at: 2025-12-10T18:01:54Z
url: https://github.com/astral-sh/ruff/pull/14122
synced_at: 2026-01-12T15:55:46Z
```

# [`flake8-bugbear`]  Handle shadowing and scope in `unused-loop-control-variable (B007)`

---

_@dylwil3_

This PR changes the logic employed to search for uses of the control variable within the loop body in order to account for shadowing and nested scope. This is achieved by using the semantic model to check whether there are any references to the binding of the control variable within the loop body.

Closes #14113 .

---

_@dylwil3 reviewed on 2024-11-06 04:17_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:105 on 2024-11-06 04:17_

If someone has a cleaner way of doing this I'd love to know!

---

_Comment by @github-actions[bot] on 2024-11-06 04:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 9 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/core.py#L991'>disnake/ext/commands/core.py:991:13:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/b23db254424692859b4d42b900728d67a4efd129/tests/utils/decorators/test_checks.py#L429'>tests/utils/decorators/test_checks.py:429:26:</a> B007 Loop control variable `val` not used within loop body
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/b23db254424692859b4d42b900728d67a4efd129/tests/utils/decorators/test_validators.py#L191'>tests/utils/decorators/test_validators.py:191:26:</a> B007 Loop control variable `val` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/airflow/api_fastapi/core_api/routes/public/dag_run.py#L128'>airflow/api_fastapi/core_api/routes/public/dag_run.py:128:20:</a> B007 Loop control variable `attr_value` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/airflow/serialization/serialized_objects.py#L973'>airflow/serialization/serialized_objects.py:973:16:</a> B007 Loop control variable `v` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L2299'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:2299:32:</a> B007 Loop control variable `param_default` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L3201'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:3201:32:</a> B007 Loop control variable `param_default` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/tests/amazon/aws/hooks/test_hooks_signature.py#L104'>providers/tests/amazon/aws/hooks/test_hooks_signature.py:104:9:</a> B007 Loop control variable `k` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/commands/tag/create.py#L91'>superset/commands/tag/create.py:91:23:</a> B007 Loop control variable `obj_id` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/23c0e810419c227455125c3cfd233c01665fe935/ibis/backends/sql/dialects.py#L291'>ibis/backends/sql/dialects.py:291:25:</a> B007 Loop control variable `e` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e4505ef501e10506780de84aa73b48a866e295e0/examples/collection.py#L131'>examples/collection.py:131:9:</a> B007 Loop control variable `i` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/fe0925b3c00bf8956a0d33408df692ac364217d4/src/pip/_internal/metadata/pkg_resources.py#L215'>src/pip/_internal/metadata/pkg_resources.py:215:17:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/e0d1a5849683e883761fc54853fa32b5b4dfcb87/reflex/state.py#L818'>reflex/state.py:818:13:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B007 | 13 | 13 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 8 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/core.py#L991'>disnake/ext/commands/core.py:991:13:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/b23db254424692859b4d42b900728d67a4efd129/tests/utils/decorators/test_checks.py#L429'>tests/utils/decorators/test_checks.py:429:26:</a> B007 Loop control variable `val` not used within loop body
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/b23db254424692859b4d42b900728d67a4efd129/tests/utils/decorators/test_validators.py#L191'>tests/utils/decorators/test_validators.py:191:26:</a> B007 Loop control variable `val` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/airflow/api_fastapi/core_api/routes/public/dag_run.py#L128'>airflow/api_fastapi/core_api/routes/public/dag_run.py:128:20:</a> B007 Loop control variable `attr_value` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/airflow/serialization/serialized_objects.py#L973'>airflow/serialization/serialized_objects.py:973:16:</a> B007 Loop control variable `v` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L2299'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:2299:32:</a> B007 Loop control variable `param_default` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L3201'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:3201:32:</a> B007 Loop control variable `param_default` not used within loop body
+ <a href='https://github.com/apache/airflow/blob/c89ab99aced32c854a7806f3a2e00f8eaeb0764a/providers/tests/amazon/aws/hooks/test_hooks_signature.py#L104'>providers/tests/amazon/aws/hooks/test_hooks_signature.py:104:9:</a> B007 Loop control variable `k` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/commands/tag/create.py#L91'>superset/commands/tag/create.py:91:23:</a> B007 Loop control variable `obj_id` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/23c0e810419c227455125c3cfd233c01665fe935/ibis/backends/sql/dialects.py#L291'>ibis/backends/sql/dialects.py:291:25:</a> B007 Loop control variable `e` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e4505ef501e10506780de84aa73b48a866e295e0/examples/collection.py#L131'>examples/collection.py:131:9:</a> B007 Loop control variable `i` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/fe0925b3c00bf8956a0d33408df692ac364217d4/src/pip/_internal/metadata/pkg_resources.py#L215'>src/pip/_internal/metadata/pkg_resources.py:215:17:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/e0d1a5849683e883761fc54853fa32b5b4dfcb87/reflex/state.py#L818'>reflex/state.py:818:13:</a> B007 Loop control variable `name` not used within loop body
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B007 | 13 | 13 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @dylwil3 on 2024-11-06 04:25_

---

_Comment by @dylwil3 on 2024-11-06 04:27_

Those ecosystem results look like they have a few false positives - let me add some more test cases for those and fix up the implementation.

The issue appears to be that the control variables are explicitly _deleted_ after or at the end of some loops, and the semantic model (correctly) removes the reference.

---

_Comment by @dylwil3 on 2025-12-10 18:01_

Closing as stale and I don't plan to get back to this in the near future. Others are welcome to pick it up!

---

_Closed by @dylwil3 on 2025-12-10 18:01_

---

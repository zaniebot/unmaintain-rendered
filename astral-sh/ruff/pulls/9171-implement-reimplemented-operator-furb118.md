```yaml
number: 9171
title: "Implement `reimplemented_operator` (FURB118)"
type: pull_request
state: merged
author: siiptuo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: FURB118
created_at: 2023-12-17T20:25:49Z
updated_at: 2023-12-18T15:16:54Z
url: https://github.com/astral-sh/ruff/pull/9171
synced_at: 2026-01-10T23:31:11Z
```

# Implement `reimplemented_operator` (FURB118)

---

_Pull request opened by @siiptuo on 2023-12-17 20:25_

## Summary

Implement [FURB118](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb118-use-operator) that recommends, for example, that `lambda x, y: x + y` is replaced with `operator.add`. Part of #1348.

## Test Plan

Added test cases.


---

_Comment by @github-actions[bot] on 2023-12-17 20:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+16 -0 violations, +0 -0 fixes in 3 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/kubernetes_tests/test_base.py#L45'>kubernetes_tests/test_base.py:45:5:</a> FURB118 Use `operator.contains` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/tests/api_internal/endpoints/test_rpc_api_endpoint.py#L63'>tests/api_internal/endpoints/test_rpc_api_endpoint.py:63:1:</a> FURB118 Use `operator.eq` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/tests/decorators/test_python.py#L657'>tests/decorators/test_python.py:657:5:</a> FURB118 Use `operator.mul` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/tests/models/test_baseoperator.py#L970'>tests/models/test_baseoperator.py:970:5:</a> FURB118 Use `operator.add` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/tests/models/test_dagrun.py#L2585'>tests/models/test_dagrun.py:2585:20:</a> FURB118 [*] Use `operator.rshift` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/43acc4ffbd97bcdaf7c4c0b47921beea72aa5bf1/tests/serialization/test_serialized_objects.py#L130'>tests/serialization/test_serialized_objects.py:130:1:</a> FURB118 Use `operator.eq` instead of defining a function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L353'>src/bokeh/core/query.py:353:9:</a> FURB118 [*] Use `operator.contains` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L354'>src/bokeh/core/query.py:354:9:</a> FURB118 [*] Use `operator.gt` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L355'>src/bokeh/core/query.py:355:9:</a> FURB118 [*] Use `operator.lt` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L356'>src/bokeh/core/query.py:356:9:</a> FURB118 [*] Use `operator.eq` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L357'>src/bokeh/core/query.py:357:9:</a> FURB118 [*] Use `operator.ge` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L358'>src/bokeh/core/query.py:358:9:</a> FURB118 [*] Use `operator.le` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L359'>src/bokeh/core/query.py:359:9:</a> FURB118 [*] Use `operator.ne` instead of defining a lambda
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/02257b8cbf637bbb32e76d2d23c16734d55cb96c/zerver/lib/management.py#L158'>zerver/lib/management.py:158:53:</a> FURB118 [*] Use `operator.or_` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/02257b8cbf637bbb32e76d2d23c16734d55cb96c/zerver/lib/mention.py#L110'>zerver/lib/mention.py:110:38:</a> FURB118 [*] Use `operator.or_` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/02257b8cbf637bbb32e76d2d23c16734d55cb96c/zerver/lib/mention.py#L160'>zerver/lib/mention.py:160:38:</a> FURB118 [*] Use `operator.or_` instead of defining a lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 16 | 16 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @charliermarsh on 2023-12-18 04:46_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-12-18 04:49_

---

_Comment by @charliermarsh on 2023-12-18 05:04_

Awesome, thank you! I'll review and merge in the morning.

---

_Label `preview` added by @charliermarsh on 2023-12-18 05:04_

---

_@charliermarsh approved on 2023-12-18 14:53_

Really nice work. Thanks!

---

_Merged by @charliermarsh on 2023-12-18 14:59_

---

_Closed by @charliermarsh on 2023-12-18 14:59_

---

_Branch deleted on 2023-12-18 15:16_

---

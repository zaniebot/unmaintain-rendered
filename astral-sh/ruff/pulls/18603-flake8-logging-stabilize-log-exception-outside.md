```yaml
number: 18603
title: "[`flake8-logging`] Stabilize `log-exception-outside-except-handler` (`LOG004`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
base: brent/release-0.12.0
head: brent/stabilize-log004
created_at: 2025-06-10T01:51:48Z
updated_at: 2025-06-12T13:04:39Z
url: https://github.com/astral-sh/ruff/pull/18603
synced_at: 2026-01-12T15:56:22Z
```

# [`flake8-logging`] Stabilize `log-exception-outside-except-handler` (`LOG004`)

---

_@ntBre_

Summary
--

Stabilizes LOG004 and updates the documentation to say explicitly that the rule still triggers even when passing the `exc_info` kwarg (https://github.com/astral-sh/ruff/issues/18044).

Test Plan
--

Existing tests, which were already in the right place

---

_Label `rule` added by @ntBre on 2025-06-10 01:51_

---

_Comment by @github-actions[bot] on 2025-06-10 01:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+25 -1 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L187'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:187:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/api_fastapi/execution_api/routes/task_instances.py#L506'>airflow-core/src/airflow/api_fastapi/execution_api/routes/task_instances.py:506:17:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/dag_processing/bundles/base.py#L373'>airflow-core/src/airflow/dag_processing/bundles/base.py:373:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/pod.py#L1338'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/pod.py:1338:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/edge3/src/airflow/providers/edge3/worker_api/auth.py#L80'>providers/edge3/src/airflow/providers/edge3/worker_api/auth.py:80:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/edge3/src/airflow/providers/edge3/worker_api/routes/_v2_routes.py#L59'>providers/edge3/src/airflow/providers/edge3/worker_api/routes/_v2_routes.py:59:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/cloud_sql.py#L1046'>providers/google/src/airflow/providers/google/cloud/operators/cloud_sql.py:1046:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L1604'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:1604:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L1760'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:1760:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L2258'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:2258:17:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/kubernetes_engine.py#L318'>providers/google/src/airflow/providers/google/cloud/operators/kubernetes_engine.py:318:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/operators/kubernetes_engine.py#L511'>providers/google/src/airflow/providers/google/cloud/operators/kubernetes_engine.py:511:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/google/src/airflow/providers/google/cloud/triggers/cloud_batch.py#L145'>providers/google/src/airflow/providers/google/cloud/triggers/cloud_batch.py:145:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/task-sdk/tests/task_sdk/definitions/test_secrets_masker.py#L136'>task-sdk/tests/task_sdk/definitions/test_secrets_masker.py:136:9:</a> LOG004 `.exception()` call outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/86e7139245e7444e749b5e8ad1d5167b4df53ce9/superset/utils/decorators.py#L231'>superset/utils/decorators.py:231:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/superset/blob/86e7139245e7444e749b5e8ad1d5167b4df53ce9/superset/views/error_handling.py#L210'>superset/views/error_handling.py:210:9:</a> LOG004 `.exception()` call outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/147fe889407481775072fec4ee391142d174b4bd/rotkehlchen/api/server.py#L436'>rotkehlchen/api/server.py:436:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `LOG004`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/decorator.py#L342'>zerver/decorator.py:342:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/decorator.py#L357'>zerver/decorator.py:357:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/decorator.py#L359'>zerver/decorator.py:359:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/decorator.py#L361'>zerver/decorator.py:361:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/email_mirror_server.py#L144'>zerver/lib/email_mirror_server.py:144:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/remote_server.py#L291'>zerver/lib/remote_server.py:291:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/worker/base.py#L269'>zerver/worker/base.py:269:17:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/worker/base.py#L271'>zerver/worker/base.py:271:17:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/worker/email_senders_base.py#L39'>zerver/worker/email_senders_base.py:39:17:</a> LOG004 `.exception()` call outside exception handlers
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG004 | 25 | 25 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-06-10 02:21_

I think this example might be a false positive:

https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/worker/email_senders_base.py#L28-L45

It looks like a closure defined in the except handler will preserve its `exc_info` in the same call stack:

```pycon
>>> import logging
>>> l = []
>>> def foo(f): f()
...
>>> try:
...     raise ValueError("my error")
... except ValueError:
...     logging.exception("building on_failure")
...     def on_failure():
...         logging.exception("on_failure called")
...     foo(on_failure)
...     l.append(on_failure)
...
ERROR:root:building on_failure
Traceback (most recent call last):
  File "<python-input-19>", line 2, in <module>
    raise ValueError("my error")
ValueError: my error
ERROR:root:on_failure called
Traceback (most recent call last):
  File "<python-input-19>", line 2, in <module>
    raise ValueError("my error")
ValueError: my error
>>> l[-1]()
ERROR:root:on_failure called
NoneType: None
```

but not if you store it somewhere else and call it later.

---

_Review requested from @dylwil3 by @ntBre on 2025-06-10 16:52_

---

_Comment by @ntBre on 2025-06-10 16:54_

What do you think about the false positive? This is the last LOG rule in preview, but my first thought was to leave it in preview even before this, so I'm happy to leave it for now.

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 20:44_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-10 20:52_

---

_Comment by @dylwil3 on 2025-06-12 13:01_

I vote to leave it in preview and address the bug!

---

_Comment by @ntBre on 2025-06-12 13:04_

Sounds good to me, thanks for opening the issue!

---

_Closed by @ntBre on 2025-06-12 13:04_

---

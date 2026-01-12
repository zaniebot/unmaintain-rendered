```yaml
number: 12796
title: "[flake8-simplify] Further simplify to binary in preview for `if-else-block-instead-of-if-exp (SIM108)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: sim108-boolean-simplify
created_at: 2024-08-10T15:44:01Z
updated_at: 2024-08-10T18:06:40Z
url: https://github.com/astral-sh/ruff/pull/12796
synced_at: 2026-01-12T15:55:42Z
```

# [flake8-simplify] Further simplify to binary in preview for `if-else-block-instead-of-if-exp (SIM108)`

---

_@dylwil3_

In most cases we should suggest a ternary operator, but there are three edge cases where a binary operator is more appropriate.

Given an if-else block of the form

```python
if test:
    target_var = body_value
else:
    target_var = else_value
```
This PR updates the check for SIM108 to the following:

- If `test == body_value` and preview enabled, suggest to replace with `target_var = test or else_value`
- If `test == not body_value` and preview enabled, suggest to replace with `target_var = body_value and else_value`
- If `not test == body_value` and preview enabled, suggest to replace with `target_var = body_value and else_value`
- Otherwise, suggest to replace with `target_var = body_value if test else else_value`

Closes #12189.


---

_Comment by @dylwil3 on 2024-08-10 15:46_

The edge cases in question are pretty rare - not sure if there is a better way to organize the code (for performance and readability) to take advantage of that.

---

_Comment by @github-actions[bot] on 2024-08-10 15:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -5 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/api/common/mark_tasks.py#L311'>airflow/api/common/mark_tasks.py:311:5:</a> SIM108 Use binary operator `start_date = dag.start_date or execution_date` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/api/common/mark_tasks.py#L311'>airflow/api/common/mark_tasks.py:311:5:</a> SIM108 Use ternary operator `start_date = dag.start_date if dag.start_date else execution_date` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/cli/commands/celery_command.py#L213'>airflow/cli/commands/celery_command.py:213:5:</a> SIM108 Use binary operator `umask = args.umask or conf.get("celery", "worker_umask", fallback=settings.DAEMON_UMASK)` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/cli/commands/celery_command.py#L213'>airflow/cli/commands/celery_command.py:213:5:</a> SIM108 Use ternary operator `umask = args.umask if args.umask else conf.get("celery", "worker_umask", fallback=settings.DAEMON_UMASK)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/celery/cli/celery_command.py#L229'>airflow/providers/celery/cli/celery_command.py:229:5:</a> SIM108 Use binary operator `umask = args.umask or conf.get("celery", "worker_umask", fallback=settings.DAEMON_UMASK)` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/celery/cli/celery_command.py#L229'>airflow/providers/celery/cli/celery_command.py:229:5:</a> SIM108 Use ternary operator `umask = args.umask if args.umask else conf.get("celery", "worker_umask", fallback=settings.DAEMON_UMASK)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L153'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:153:13:</a> SIM108 Use binary operator `namespaces = self.kube_config.multi_namespace_mode_namespace_list or [None]` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/databricks/operators/databricks_sql.py#L139'>airflow/providers/databricks/operators/databricks_sql.py:139:17:</a> SIM108 Use binary operator `csv_params = self._csv_params or {}` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/databricks/operators/databricks_sql.py#L139'>airflow/providers/databricks/operators/databricks_sql.py:139:17:</a> SIM108 Use ternary operator `csv_params = self._csv_params if self._csv_params else {}` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/imap/hooks/imap.py#L91'>airflow/providers/imap/hooks/imap.py:91:13:</a> SIM108 Use binary operator `ssl_context_string = extra_ssl_context or conf.get("imap", "SSL_CONTEXT", fallback=None)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/b87f9878ff801575b35a30220c0b2e285a2cf1b8/airflow/providers/smtp/hooks/smtp.py#L118'>airflow/providers/smtp/hooks/smtp.py:118:13:</a> SIM108 Use binary operator `ssl_context_string = extra_ssl_context or conf.get("smtp_provider", "SSL_CONTEXT", fallback=None)` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/skbuild/setuptools_wrap.py#L532'>skbuild/setuptools_wrap.py:532:5:</a> SIM108 Use binary operator `cmake_install_target = cmake_install_target_from_command or cmake_install_target_from_setup` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/webhooks/bitbucket3/view.py#L78'>zerver/webhooks/bitbucket3/view.py:78:5:</a> SIM108 Use binary operator `topic_name = include_title or "Bitbucket Server Ping"` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/webhooks/bitbucket3/view.py#L78'>zerver/webhooks/bitbucket3/view.py:78:5:</a> SIM108 Use ternary operator `topic_name = include_title if include_title else "Bitbucket Server Ping"` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 14 | 9 | 5 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-08-10 16:17_

Sweet, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-10 16:32_

---

_Label `rule` added by @charliermarsh on 2024-08-10 16:32_

---

_Label `preview` added by @charliermarsh on 2024-08-10 16:32_

---

_@charliermarsh approved on 2024-08-10 16:41_

This looks good to me. I just added a check to ensure there's no effect (like a function call) in the test, since it no longer gets repeated.

---

_Merged by @charliermarsh on 2024-08-10 16:49_

---

_Closed by @charliermarsh on 2024-08-10 16:49_

---

_Branch deleted on 2024-08-10 18:06_

---

```yaml
number: 12346
title: Consider expression before statement when determining binding kind
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/expr-stmt
created_at: 2024-07-16T14:43:51Z
updated_at: 2024-07-16T15:00:41Z
url: https://github.com/astral-sh/ruff/pull/12346
synced_at: 2026-01-12T15:55:41Z
```

# Consider expression before statement when determining binding kind

---

_@charliermarsh_

## Summary

I believe these should always bind more tightly -- e.g., in:

```python
for _ in bar(baz for foo in [1]):
    pass
```

The inner `baz` and `foo` should be considered comprehension variables, not for loop bindings.

We need to revisit this more holistically. In some of these cases, `BindingKind` should probably be a flag, not an enum, since the values aren't mutually exclusive. Separately, we should probably be more precise in how we set it (e.g., by passing down from the parent rather than sniffing in `handle_node_store`).

Closes https://github.com/astral-sh/ruff/issues/12339


---

_Label `bug` added by @charliermarsh on 2024-07-16 14:49_

---

_Merged by @charliermarsh on 2024-07-16 14:49_

---

_Closed by @charliermarsh on 2024-07-16 14:49_

---

_Branch deleted on 2024-07-16 14:49_

---

_Comment by @github-actions[bot] on 2024-07-16 14:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ee2be505ec25eb26a7928a5a5ae2b04c7efa8513/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L689'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:689:51:</a> PLR1704 Redefining argument with the local name `python`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1704 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ee2be505ec25eb26a7928a5a5ae2b04c7efa8513/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L689'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:689:51:</a> PLR1704 Redefining argument with the local name `python`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1704 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-07-16 15:00_

Ecosystem change is correct.

---

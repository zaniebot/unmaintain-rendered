```yaml
number: 10890
title: "[`pylint`] Reverse min-max logic in `if-stmt-min-max`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-04-11T17:45:55Z
updated_at: 2024-04-11T18:16:14Z
url: https://github.com/astral-sh/ruff/pull/10890
synced_at: 2026-01-10T22:37:01Z
```

# [`pylint`] Reverse min-max logic in `if-stmt-min-max`

---

_Pull request opened by @charliermarsh on 2024-04-11 17:45_

Closes https://github.com/astral-sh/ruff/issues/10889.

---

_Label `bug` added by @charliermarsh on 2024-04-11 17:46_

---

_Renamed from "Reverse min-max logic in if-stmt-min-max" to "[`pylint`] Reverse min-max logic in `if-stmt-min-max`" by @charliermarsh on 2024-04-11 17:46_

---

_Comment by @github-actions[bot] on 2024-04-11 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/airflow/cli/commands/kubernetes_command.py#L84'>airflow/cli/commands/kubernetes_command.py:84:5:</a> PLR1730 [*] Replace `if` statement with `min_pending_minutes = max(min_pending_minutes, 5)`
- <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/airflow/cli/commands/kubernetes_command.py#L84'>airflow/cli/commands/kubernetes_command.py:84:5:</a> PLR1730 [*] Replace `if` statement with `min_pending_minutes = min(min_pending_minutes, 5)`
+ <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/airflow/models/taskinstance.py#L2171'>airflow/models/taskinstance.py:2171:13:</a> PLR1730 [*] Replace `if` statement with `min_backoff = max(min_backoff, 1)`
- <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/airflow/models/taskinstance.py#L2171'>airflow/models/taskinstance.py:2171:13:</a> PLR1730 [*] Replace `if` statement with `min_backoff = min(min_backoff, 1)`
- <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/tests/cluster_policies/__init__.py#L103'>tests/cluster_policies/__init__.py:103:5:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/apache/airflow/blob/f3ab31de97306b6eb17c38e42650fc796b7f9d64/tests/cluster_policies/__init__.py#L103'>tests/cluster_policies/__init__.py:103:5:</a> PLR1730 [*] Replace `if` statement with `min` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/bulk_writer/buffer.py#L234'>pymilvus/bulk_writer/buffer.py:234:13:</a> PLR1730 [*] Replace `if` statement with `max` call
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/bulk_writer/buffer.py#L234'>pymilvus/bulk_writer/buffer.py:234:13:</a> PLR1730 [*] Replace `if` statement with `min` call
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/bulk_writer/buffer.py#L236'>pymilvus/bulk_writer/buffer.py:236:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/bulk_writer/buffer.py#L236'>pymilvus/bulk_writer/buffer.py:236:13:</a> PLR1730 [*] Replace `if` statement with `min` call
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/iterator.py#L54'>pymilvus/orm/iterator.py:54:9:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/iterator.py#L54'>pymilvus/orm/iterator.py:54:9:</a> PLR1730 [*] Replace `if` statement with `min` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/exchanges/bybit.py#L403'>rotkehlchen/exchanges/bybit.py:403:13:</a> PLR1730 [*] Replace `if` statement with `lower_ts = max(lower_ts, start_ts)`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/exchanges/bybit.py#L403'>rotkehlchen/exchanges/bybit.py:403:13:</a> PLR1730 [*] Replace `if` statement with `lower_ts = min(lower_ts, start_ts)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1730 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-11 18:16_

---

_Closed by @charliermarsh on 2024-04-11 18:16_

---

_Branch deleted on 2024-04-11 18:16_

---

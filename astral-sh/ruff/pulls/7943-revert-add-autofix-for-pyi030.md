```yaml
number: 7943
title: "Revert \"add autofix for `PYI030`\""
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: revert/7880
created_at: 2023-10-13T13:54:23Z
updated_at: 2023-10-13T14:24:49Z
url: https://github.com/astral-sh/ruff/pull/7943
synced_at: 2026-01-12T02:32:41Z
```

# Revert "add autofix for `PYI030`"

---

_Pull request opened by @zanieb on 2023-10-13 13:54_

This reverts commit #7880 (d8c0360fc7cda384821c0aab125f9757a230181e) which does not perform the correct fix per https://github.com/astral-sh/ruff/pull/7934


---

_Renamed from "Revert "add autofix for `PYI030` (#7880)"" to "Revert "add autofix for `PYI030`"" by @zanieb on 2023-10-13 13:55_

---

_Comment by @github-actions[bot] on 2023-10-13 14:08_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
+ [*] 16069 fixable with the `--fix` option (6167 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 16072 fixable with the `--fix` option (6167 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
- <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
+ <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/models/mappedoperator.py#L85'>airflow/models/mappedoperator.py:85:20:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
- <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/models/mappedoperator.py#L85'>airflow/models/mappedoperator.py:85:20:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
+ <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
- <a href='https://github.com/apache/airflow/blob/aad573e6860a3ba4fad23b45fa792cd0f8e9f0c3/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
+ [*] 17800 fixable with the `--fix` option (4369 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 17801 fixable with the `--fix` option (4369 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/serialization.py#L149'>src/bokeh/core/serialization.py:149:25:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["bool", "object"]`
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/serialization.py#L149'>src/bokeh/core/serialization.py:149:25:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["bool", "object"]`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI030 | 8 | 4 | 4 |



---

_Merged by @zanieb on 2023-10-13 14:24_

---

_Closed by @zanieb on 2023-10-13 14:24_

---

_Branch deleted on 2023-10-13 14:24_

---

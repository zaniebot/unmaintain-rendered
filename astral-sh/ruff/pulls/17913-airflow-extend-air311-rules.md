```yaml
number: 17913
title: "[`airflow`] extend `AIR311` rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR311
created_at: 2025-05-07T12:47:06Z
updated_at: 2025-05-09T17:08:38Z
url: https://github.com/astral-sh/ruff/pull/17913
synced_at: 2026-01-10T18:57:03Z
```

# [`airflow`] extend `AIR311` rules

---

_Pull request opened by @Lee-W on 2025-05-07 12:47_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* `airflow.models.Connection` → `airflow.sdk.Connection`
* `airflow.models.Variable` → `airflow.sdk.Variable`

## Test Plan

<!-- How was it tested? -->

The test fixtures has been updated (see the first commit for easier review)


---

_Comment by @github-actions[bot] on 2025-05-07 12:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+952 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+952 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L115'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:115:17:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L125'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:125:26:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L154'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:154:18:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L193'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:193:17:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L193'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:193:52:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L232'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:232:16:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L72'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:72:40:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L92'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:92:40:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L115'>airflow-core/src/airflow/cli/commands/connection_command.py:115:37:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L122'>airflow-core/src/airflow/cli/commands/connection_command.py:122:27:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L191'>airflow-core/src/airflow/cli/commands/connection_command.py:191:50:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L191'>airflow-core/src/airflow/cli/commands/connection_command.py:191:71:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L258'>airflow-core/src/airflow/cli/commands/connection_command.py:258:20:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L262'>airflow-core/src/airflow/cli/commands/connection_command.py:262:20:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L266'>airflow-core/src/airflow/cli/commands/connection_command.py:266:20:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L280'>airflow-core/src/airflow/cli/commands/connection_command.py:280:38:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L280'>airflow-core/src/airflow/cli/commands/connection_command.py:280:56:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L309'>airflow-core/src/airflow/cli/commands/connection_command.py:309:48:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L309'>airflow-core/src/airflow/cli/commands/connection_command.py:309:66:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L344'>airflow-core/src/airflow/cli/commands/connection_command.py:344:54:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L344'>airflow-core/src/airflow/cli/commands/connection_command.py:344:75:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L47'>airflow-core/src/airflow/cli/commands/connection_command.py:47:30:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L85'>airflow-core/src/airflow/cli/commands/connection_command.py:85:24:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L87'>airflow-core/src/airflow/cli/commands/connection_command.py:87:33:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/connection_command.py#L98'>airflow-core/src/airflow/cli/commands/connection_command.py:98:31:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L40'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:40:17:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L41'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:41:34:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L41'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:41:60:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L44'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:44:36:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L44'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:44:63:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L106'>airflow-core/src/airflow/cli/commands/variable_command.py:106:13:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L126'>airflow-core/src/airflow/cli/commands/variable_command.py:126:38:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L42'>airflow-core/src/airflow/cli/commands/variable_command.py:42:44:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L52'>airflow-core/src/airflow/cli/commands/variable_command.py:52:19:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L55'>airflow-core/src/airflow/cli/commands/variable_command.py:55:19:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L65'>airflow-core/src/airflow/cli/commands/variable_command.py:65:5:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L73'>airflow-core/src/airflow/cli/commands/variable_command.py:73:5:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L94'>airflow-core/src/airflow/cli/commands/variable_command.py:94:52:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/cli/commands/variable_command.py#L94'>airflow-core/src/airflow/cli/commands/variable_command.py:94:72:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/secrets/metastore.py#L39'>airflow-core/src/airflow/secrets/metastore.py:39:79:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/secrets/metastore.py#L49'>airflow-core/src/airflow/secrets/metastore.py:49:38:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/secrets/metastore.py#L49'>airflow-core/src/airflow/secrets/metastore.py:49:56:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/secrets/metastore.py#L64'>airflow-core/src/airflow/secrets/metastore.py:64:43:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/src/airflow/secrets/metastore.py#L64'>airflow-core/src/airflow/secrets/metastore.py:64:59:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L119'>airflow-core/tests/unit/always/test_connection.py:119:27:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L128'>airflow-core/tests/unit/always/test_connection.py:128:27:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L140'>airflow-core/tests/unit/always/test_connection.py:140:31:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L351'>airflow-core/tests/unit/always/test_connection.py:351:22:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L381'>airflow-core/tests/unit/always/test_connection.py:381:22:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/7688825507e422d122da332a87ef712b1a54c84e/airflow-core/tests/unit/always/test_connection.py#L383'>airflow-core/tests/unit/always/test_connection.py:383:20:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 902 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 952 | 952 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @MichaReiser on 2025-05-09 06:29_

---

_@ntBre approved on 2025-05-09 15:15_

LGTM!

---

_Label `rule` added by @ntBre on 2025-05-09 15:17_

---

_Label `preview` added by @ntBre on 2025-05-09 15:17_

---

_Merged by @ntBre on 2025-05-09 17:08_

---

_Closed by @ntBre on 2025-05-09 17:08_

---

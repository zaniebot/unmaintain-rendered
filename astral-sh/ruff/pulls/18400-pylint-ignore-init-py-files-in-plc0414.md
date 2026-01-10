```yaml
number: 18400
title: "[`pylint`] Ignore __init__.py files in (PLC0414)"
type: pull_request
state: merged
author: GideonBear
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-f401-plc0414-conflict
created_at: 2025-05-31T08:34:46Z
updated_at: 2025-06-21T06:49:15Z
url: https://github.com/astral-sh/ruff/pull/18400
synced_at: 2026-01-10T18:39:08Z
```

# [`pylint`] Ignore __init__.py files in (PLC0414)

---

_Pull request opened by @GideonBear on 2025-05-31 08:34_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Ignore `__init__.py` files in `useless-import-alias` (PLC0414).
See discussion in #18365 and #6294: we want to allow redundant aliases in `__init__.py` files, as they're almost always intentional explicit re-exports.
Closes #18365
 Closes #6294
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
I added a new snapshot test.

---

_Renamed from "[pylint] Ignore __init__.py files in (PLC0414)" to "[`pylint`] Ignore __init__.py files in (PLC0414)" by @GideonBear on 2025-05-31 09:36_

---

_Review requested from @ntBre by @ntBre on 2025-05-31 14:05_

---

_Label `bug` added by @ntBre on 2025-05-31 14:05_

---

_Comment by @github-actions[bot] on 2025-05-31 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -89 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L20'>airflow-core/src/airflow/decorators/__init__.py:20:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L21'>airflow-core/src/airflow/decorators/__init__.py:21:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L22'>airflow-core/src/airflow/decorators/__init__.py:22:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L23'>airflow-core/src/airflow/decorators/__init__.py:23:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L24'>airflow-core/src/airflow/decorators/__init__.py:24:5:</a> PLC0414 Import alias does not rename original package
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -84 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L10'>corporate/models/__init__.py:10:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L11'>corporate/models/__init__.py:11:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L1'>corporate/models/__init__.py:1:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L2'>corporate/models/__init__.py:2:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L3'>corporate/models/__init__.py:3:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L4'>corporate/models/__init__.py:4:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L5'>corporate/models/__init__.py:5:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L6'>corporate/models/__init__.py:6:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L7'>corporate/models/__init__.py:7:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L8'>corporate/models/__init__.py:8:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L9'>corporate/models/__init__.py:9:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L10'>zerver/models/__init__.py:10:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L11'>zerver/models/__init__.py:11:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L12'>zerver/models/__init__.py:12:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L13'>zerver/models/__init__.py:13:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L14'>zerver/models/__init__.py:14:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L15'>zerver/models/__init__.py:15:38:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L16'>zerver/models/__init__.py:16:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L17'>zerver/models/__init__.py:17:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L18'>zerver/models/__init__.py:18:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L19'>zerver/models/__init__.py:19:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L1'>zerver/models/__init__.py:1:27:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L20'>zerver/models/__init__.py:20:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L21'>zerver/models/__init__.py:21:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L22'>zerver/models/__init__.py:22:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L23'>zerver/models/__init__.py:23:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L24'>zerver/models/__init__.py:24:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L25'>zerver/models/__init__.py:25:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L26'>zerver/models/__init__.py:26:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L27'>zerver/models/__init__.py:27:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L28'>zerver/models/__init__.py:28:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L29'>zerver/models/__init__.py:29:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L2'>zerver/models/__init__.py:2:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L30'>zerver/models/__init__.py:30:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L31'>zerver/models/__init__.py:31:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L32'>zerver/models/__init__.py:32:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L33'>zerver/models/__init__.py:33:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L34'>zerver/models/__init__.py:34:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L35'>zerver/models/__init__.py:35:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L36'>zerver/models/__init__.py:36:44:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L37'>zerver/models/__init__.py:37:44:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L38'>zerver/models/__init__.py:38:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L39'>zerver/models/__init__.py:39:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L3'>zerver/models/__init__.py:3:32:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L40'>zerver/models/__init__.py:40:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L41'>zerver/models/__init__.py:41:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L42'>zerver/models/__init__.py:42:40:</a> PLC0414 Import alias does not rename original package
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0414 | 89 | 0 | 89 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -89 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L20'>airflow-core/src/airflow/decorators/__init__.py:20:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L21'>airflow-core/src/airflow/decorators/__init__.py:21:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L22'>airflow-core/src/airflow/decorators/__init__.py:22:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L23'>airflow-core/src/airflow/decorators/__init__.py:23:5:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/src/airflow/decorators/__init__.py#L24'>airflow-core/src/airflow/decorators/__init__.py:24:5:</a> PLC0414 Import alias does not rename original package
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -84 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L10'>corporate/models/__init__.py:10:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L11'>corporate/models/__init__.py:11:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L1'>corporate/models/__init__.py:1:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L2'>corporate/models/__init__.py:2:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L3'>corporate/models/__init__.py:3:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L4'>corporate/models/__init__.py:4:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L5'>corporate/models/__init__.py:5:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L6'>corporate/models/__init__.py:6:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L7'>corporate/models/__init__.py:7:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L8'>corporate/models/__init__.py:8:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/corporate/models/__init__.py#L9'>corporate/models/__init__.py:9:43:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L10'>zerver/models/__init__.py:10:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L11'>zerver/models/__init__.py:11:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L12'>zerver/models/__init__.py:12:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L13'>zerver/models/__init__.py:13:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L14'>zerver/models/__init__.py:14:34:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L15'>zerver/models/__init__.py:15:38:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L16'>zerver/models/__init__.py:16:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L17'>zerver/models/__init__.py:17:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L18'>zerver/models/__init__.py:18:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L19'>zerver/models/__init__.py:19:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L1'>zerver/models/__init__.py:1:27:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L20'>zerver/models/__init__.py:20:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L21'>zerver/models/__init__.py:21:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L22'>zerver/models/__init__.py:22:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L23'>zerver/models/__init__.py:23:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L24'>zerver/models/__init__.py:24:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L25'>zerver/models/__init__.py:25:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L26'>zerver/models/__init__.py:26:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L27'>zerver/models/__init__.py:27:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L28'>zerver/models/__init__.py:28:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L29'>zerver/models/__init__.py:29:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L2'>zerver/models/__init__.py:2:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L30'>zerver/models/__init__.py:30:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L31'>zerver/models/__init__.py:31:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L32'>zerver/models/__init__.py:32:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L33'>zerver/models/__init__.py:33:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L34'>zerver/models/__init__.py:34:36:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L35'>zerver/models/__init__.py:35:39:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L36'>zerver/models/__init__.py:36:44:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L37'>zerver/models/__init__.py:37:44:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L38'>zerver/models/__init__.py:38:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L39'>zerver/models/__init__.py:39:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L3'>zerver/models/__init__.py:3:32:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L40'>zerver/models/__init__.py:40:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L41'>zerver/models/__init__.py:41:40:</a> PLC0414 Import alias does not rename original package
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/models/__init__.py#L42'>zerver/models/__init__.py:42:40:</a> PLC0414 Import alias does not rename original package
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0414 | 89 | 0 | 89 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-06-20 06:37_

@ntBre or @dylwil3 could either of you review this PR.

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/import_aliasing_2/__init__.py`:3 on 2025-06-20 15:22_

I'm not sure what these comments mean - could we remove them?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:81 on 2025-06-20 15:23_

```suggestion
    if checker.path().ends_with("__init__.py") {
        return;
    }
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:120 on 2025-06-20 15:24_

```suggestion
    if checker.path().ends_with("__init__.py") {
        return;
    }
```

---

_@dylwil3 requested changes on 2025-06-20 15:26_

Thanks! A couple nits

---

_@GideonBear reviewed on 2025-06-20 15:43_

---

_Review comment by @GideonBear on `crates/ruff_linter/resources/test/fixtures/pylint/import_aliasing_2/__init__.py`:3 on 2025-06-20 15:43_

I copied these from another useless-import-alias test. The ignores are (I assume) to make sure the snapshot test only contains the messages we want to be testing.
Lines 2 and 3 I thought were standard? I can remove them if you want.

---

_@dylwil3 reviewed on 2025-06-20 15:55_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/import_aliasing_2/__init__.py`:3 on 2025-06-20 15:55_

I think probably the fixture was over-eagerly copied from pylint? those ignores do nothing for ruff :) let's remove these

---

_@GideonBear reviewed on 2025-06-20 16:34_

---

_Review comment by @GideonBear on `crates/ruff_linter/resources/test/fixtures/pylint/import_aliasing_2/__init__.py`:3 on 2025-06-20 16:34_

Shoot, I completely overlooked that; I thought these were normal ruff ignores! I'll remove them here; but should they be removed from other tests as well?

---

_@dylwil3 reviewed on 2025-06-20 18:02_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/import_aliasing_2/__init__.py`:3 on 2025-06-20 18:02_

No worries; we don't have to cleanup all the other comments in fixtures in this PR. It would be a big undertaking!

---

_@dylwil3 approved on 2025-06-20 18:03_

---

_Label `bug` removed by @dylwil3 on 2025-06-20 18:15_

---

_Label `rule` added by @dylwil3 on 2025-06-20 18:15_

---

_Label `preview` added by @dylwil3 on 2025-06-20 18:15_

---

_Comment by @dylwil3 on 2025-06-20 18:17_

On second thought I decided to gate this behind preview for now. (I think technically we don't have to since it reduces the number of lints, but it seems like a big enough change that I think it's a good idea).

---

_Comment by @ntBre on 2025-06-20 18:20_

Thank you both!

---

_Merged by @dylwil3 on 2025-06-20 18:20_

---

_Closed by @dylwil3 on 2025-06-20 18:20_

---

_Branch deleted on 2025-06-21 06:49_

---

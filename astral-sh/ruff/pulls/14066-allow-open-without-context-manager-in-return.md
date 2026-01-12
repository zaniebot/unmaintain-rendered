```yaml
number: 14066
title: "Allow `open` without context manager in `return` statement"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-11-03T19:00:51Z
updated_at: 2024-11-03T19:16:29Z
url: https://github.com/astral-sh/ruff/pull/14066
synced_at: 2026-01-12T15:55:46Z
```

# Allow `open` without context manager in `return` statement

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/13862.


---

_Renamed from "Allow open without context manager in return statement" to "Allow `open` without context manager in `return` statement" by @charliermarsh on 2024-11-03 19:00_

---

_Label `bug` added by @charliermarsh on 2024-11-03 19:00_

---

_Marked ready for review by @charliermarsh on 2024-11-03 19:01_

---

_Comment by @github-actions[bot] on 2024-11-03 19:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -6 violations, +0 -0 fixes in 5 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/utils/file.py#L159'>airflow/utils/file.py:159:30:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L820'>src/bokeh/settings.py:820:34:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/sampledata.py#L158'>src/bokeh/util/sampledata.py:158:12:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/scripts/lib/zulip_tools.py#L478'>scripts/lib/zulip_tools.py:478:36:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L267'>zerver/lib/test_helpers.py:267:65:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L782'>zerver/lib/test_helpers.py:782:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L782'>zerver/lib/test_helpers.py:782:5:</a> D415 First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/views/upload.py#L267'>zerver/views/upload.py:267:82:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/056dc8a2b720b5e3989d99936a6d9ba47115911b/indico/core/storage/backend.py#L231'>indico/core/storage/backend.py:231:61:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM115 | 5 | 0 | 5 | 0 | 0 |
| RUF100 | 4 | 4 | 0 | 0 | 0 |
| D415 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -6 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/utils/file.py#L159'>airflow/utils/file.py:159:30:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/ff6038b8d154ccdde65a4ebd652ad41d88e8c33e/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L820'>src/bokeh/settings.py:820:34:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/sampledata.py#L158'>src/bokeh/util/sampledata.py:158:12:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/scripts/lib/zulip_tools.py#L478'>scripts/lib/zulip_tools.py:478:36:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L267'>zerver/lib/test_helpers.py:267:65:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L782'>zerver/lib/test_helpers.py:782:5:</a> D400 First line should end with a period
- <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/lib/test_helpers.py#L782'>zerver/lib/test_helpers.py:782:5:</a> D400 First line should end with a period
+ <a href='https://github.com/zulip/zulip/blob/d556c0e0a5e9e8ba0ff548354bf2ba6ef2c97d4b/zerver/views/upload.py#L267'>zerver/views/upload.py:267:82:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/056dc8a2b720b5e3989d99936a6d9ba47115911b/indico/core/storage/backend.py#L231'>indico/core/storage/backend.py:231:61:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM115`)
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM115 | 5 | 0 | 5 | 0 | 0 |
| RUF100 | 4 | 4 | 0 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-11-03 19:16_

---

_Closed by @charliermarsh on 2024-11-03 19:16_

---

_Branch deleted on 2024-11-03 19:16_

---

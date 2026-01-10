```yaml
number: 18518
title: "[`flake8-pyi`] Stabilize autofix for `future-annotations-in-stub` (`PYI044`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-autofix-future
created_at: 2025-06-06T23:05:31Z
updated_at: 2025-06-08T18:05:19Z
url: https://github.com/astral-sh/ruff/pull/18518
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-pyi`] Stabilize autofix for `future-annotations-in-stub` (`PYI044`)

---

_Pull request opened by @dylwil3 on 2025-06-06 23:05_

_No description provided._

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-06-06 23:05_

---

_Label `rule` added by @dylwil3 on 2025-06-06 23:05_

---

_Label `fixes` added by @dylwil3 on 2025-06-06 23:05_

---

_Comment by @github-actions[bot] on 2025-06-06 23:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +8 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/migrations/db_types.pyi#L19'>airflow-core/src/airflow/migrations/db_types.pyi:19:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/migrations/db_types.pyi#L19'>airflow-core/src/airflow/migrations/db_types.pyi:19:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi#L21'>task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi:21:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi#L21'>task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi:21:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L1'>src/typings/selenium/webdriver/common/action_chains.pyi:1:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L1'>src/typings/selenium/webdriver/common/action_chains.pyi:1:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L1'>src/typings/selenium/webdriver/remote/webelement.pyi:1:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L1'>src/typings/selenium/webdriver/remote/webelement.pyi:1:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI044 | 8 | 0 | 0 | 8 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 23:28_

---

_@AlexWaygood approved on 2025-06-07 17:07_

---

_Merged by @dylwil3 on 2025-06-08 18:05_

---

_Closed by @dylwil3 on 2025-06-08 18:05_

---

_Branch deleted on 2025-06-08 18:05_

---

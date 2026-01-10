```yaml
number: 12676
title: "[`flake8-pyi`] - add autofix for `future-annotations-in-stub` (`PYI044`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: autofix-pyi044
created_at: 2024-08-05T04:21:56Z
updated_at: 2024-08-06T02:28:04Z
url: https://github.com/astral-sh/ruff/pull/12676
synced_at: 2026-01-10T21:47:02Z
```

# [`flake8-pyi`] - add autofix for `future-annotations-in-stub` (`PYI044`)

---

_Pull request opened by @diceroll123 on 2024-08-05 04:21_

## Summary

add autofix for `PYI044`

## Test Plan

`cargo test`

---

_Review requested from @AlexWaygood by @diceroll123 on 2024-08-05 04:21_

---

_Comment by @github-actions[bot] on 2024-08-05 04:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +10 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/decorators/__init__.pyi#L21'>airflow/decorators/__init__.pyi:21:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/decorators/__init__.pyi#L21'>airflow/decorators/__init__.pyi:21:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/migrations/db_types.pyi#L19'>airflow/migrations/db_types.pyi:19:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/migrations/db_types.pyi#L19'>airflow/migrations/db_types.pyi:19:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/utils/context.pyi#L27'>airflow/utils/context.pyi:27:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/c55438d9b2eb9b6680641eefdd0cbc67a28d1d29/airflow/utils/context.pyi#L27'>airflow/utils/context.pyi:27:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
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
| PYI044 | 10 | 0 | 0 | 10 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/future_annotations_in_stub.rs`:32 on 2024-08-05 07:57_

```suggestion
        Some("Remove `from __future__ import annotations`".to_string())
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/future_annotations_in_stub.rs`:61 on 2024-08-05 07:58_

Arguably I think this is a safe edit. The premise of the rule is that `from __future__ import annotations` has literally no effect in stub files, so on that basis it can't be unsafe to remove the redundant import

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI044.pyi`:3 on 2024-08-05 07:59_

nit: let's use another import that actually exists in the `__future__` module ;)

```suggestion
from __future__ import annotations, with_statement # PYI044.
```

---

_@AlexWaygood reviewed on 2024-08-05 07:59_

Nice! Looks good overall

---

_Comment by @codspeed-hq[bot] on 2024-08-05 23:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:autofix-pyi044)

### Merging #12676 will **not alter performance**

<sub>Comparing <code>diceroll123:autofix-pyi044</code> (cb3495c) with <code>main</code> (5499821)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review requested from @AlexWaygood by @diceroll123 on 2024-08-05 23:44_

---

_@charliermarsh approved on 2024-08-06 02:27_

---

_Label `fixes` added by @charliermarsh on 2024-08-06 02:27_

---

_Label `preview` added by @charliermarsh on 2024-08-06 02:27_

---

_Merged by @charliermarsh on 2024-08-06 02:27_

---

_Closed by @charliermarsh on 2024-08-06 02:27_

---

_Comment by @charliermarsh on 2024-08-06 02:28_

Thanks @AlexWaygood for the review.

---

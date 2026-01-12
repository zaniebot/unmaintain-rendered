```yaml
number: 9268
title: "[`pylint`] - implement `redeclared-assigned-name` (`W0128`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-W0128
created_at: 2023-12-24T22:54:52Z
updated_at: 2024-03-15T14:44:02Z
url: https://github.com/astral-sh/ruff/pull/9268
synced_at: 2026-01-12T15:55:28Z
```

# [`pylint`] - implement `redeclared-assigned-name` (`W0128`)

---

_@diceroll123_

## Summary

Implements [`W0128`/`redeclared-assigned-name`](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/redeclared-assigned-name.html)

See: #970 

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-12-24 23:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/00cd44e4ccb4a7d6406e75d3cf8f80277545d5d2/tests/providers/common/sql/operators/test_sql.py#L237'>tests/providers/common/sql/operators/test_sql.py:237:20:</a> PLW0128 Redeclared variable `operator` in assignment
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0128 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[pylint] - implement W0128/redeclared-assigned-name" to "[pylint] - implement `W0128`/`redeclared-assigned-name`" by @diceroll123 on 2023-12-25 00:05_

---

_Renamed from "[pylint] - implement `W0128`/`redeclared-assigned-name`" to "[`pylint`] - implement `W0128`/`redeclared-assigned-name`" by @diceroll123 on 2024-01-20 22:56_

---

_Renamed from "[`pylint`] - implement `W0128`/`redeclared-assigned-name`" to "[`pylint`] - implement `redeclared-assigned-name` (`W0128`)" by @diceroll123 on 2024-01-21 04:32_

---

_@zanieb reviewed on 2024-03-13 14:57_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/redeclared_assigned_name.rs`:16 on 2024-03-13 14:57_

Can you add an example to the docs?

---

_@zanieb approved on 2024-03-13 14:58_

Nice! Just needs that small docs change

---

_Comment by @codspeed-hq[bot] on 2024-03-15 03:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-W0128)

### Merging #9268 will **not alter performance**

<sub>Comparing <code>diceroll123:add-W0128</code> (ab12b93) with <code>main</code> (9675e18)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @diceroll123 on 2024-03-15 04:32_

Updated accordingly, and added support for the dummy variable setting instead of just using `_`. 😄 

---

_Review requested from @zanieb by @diceroll123 on 2024-03-15 04:32_

---

_@zanieb approved on 2024-03-15 14:43_

---

_Merged by @zanieb on 2024-03-15 14:43_

---

_Closed by @zanieb on 2024-03-15 14:43_

---

_Label `rule` added by @zanieb on 2024-03-15 14:44_

---

_Label `preview` added by @zanieb on 2024-03-15 14:44_

---

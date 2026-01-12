```yaml
number: 9520
title: "[`tryceratops`] Add fix for `error-instead-of-exception` (`TRY400`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-autofix-for-TRY400
created_at: 2024-01-15T03:19:49Z
updated_at: 2024-01-16T03:08:17Z
url: https://github.com/astral-sh/ruff/pull/9520
synced_at: 2026-01-12T15:55:29Z
```

# [`tryceratops`] Add fix for `error-instead-of-exception` (`TRY400`)

---

_@diceroll123_

## Summary

add autofix for `error-instead-of-exception` (`TRY400`)

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-15 03:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +4 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/sensors/base.py#L260'>airflow/sensors/base.py:260:21:</a> TRY400 Use `logging.exception` instead of `logging.error`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/sensors/base.py#L260'>airflow/sensors/base.py:260:21:</a> TRY400 [*] Use `logging.exception` instead of `logging.error`
- <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/www/extensions/init_jinja_globals.py#L54'>airflow/www/extensions/init_jinja_globals.py:54:9:</a> TRY400 Use `logging.exception` instead of `logging.error`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/www/extensions/init_jinja_globals.py#L54'>airflow/www/extensions/init_jinja_globals.py:54:9:</a> TRY400 [*] Use `logging.exception` instead of `logging.error`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY400 | 4 | 0 | 0 | 4 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-01-16 02:31_

Great, thank you!

---

_Renamed from "[`tryceratops`] - add fix for `error-instead-of-exception` (`TRY400`)" to "[`tryceratops`] Add fix for `error-instead-of-exception` (`TRY400`)" by @charliermarsh on 2024-01-16 02:33_

---

_Label `autofix` added by @charliermarsh on 2024-01-16 02:33_

---

_Label `preview` added by @charliermarsh on 2024-01-16 02:33_

---

_Merged by @charliermarsh on 2024-01-16 03:00_

---

_Closed by @charliermarsh on 2024-01-16 03:00_

---

_Comment by @codspeed-hq[bot] on 2024-01-16 03:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-autofix-for-TRY400)

### Merging #9520 will **degrade performances by 4.35%**

<sub>Comparing <code>diceroll123:add-autofix-for-TRY400</code> (47b4422) with <code>main</code> (f9331c7)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-autofix-for-TRY400)._

### Benchmarks breakdown

|     | Benchmark | `main` | `diceroll123:add-autofix-for-TRY400` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

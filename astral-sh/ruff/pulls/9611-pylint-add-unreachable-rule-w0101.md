```yaml
number: 9611
title: "[`pylint`] - Add `unreachable` rule (`W0101`)"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
draft: true
base: main
head: add-PLW0101
created_at: 2024-01-22T08:29:30Z
updated_at: 2024-03-28T17:30:57Z
url: https://github.com/astral-sh/ruff/pull/9611
synced_at: 2026-01-12T15:55:29Z
```

# [`pylint`] - Add `unreachable` rule (`W0101`)

---

_@diceroll123_

## Summary

Add `unreachable`, shows where the end of the current body's unreachable code is.

See: #970

## Test Plan

`cargo test` _and a lot of crying because of late night recursion woes_

---

_Comment by @github-actions[bot] on 2024-01-22 08:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+14 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/providers/google/cloud/operators/datafusion.py#L53'>airflow/providers/google/cloud/operators/datafusion.py:53:9:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/triggers/base.py#L75'>airflow/triggers/base.py:75:9:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/dev/example_dags/update_example_dags_paths.py#L49'>dev/example_dags/update_example_dags_paths.py:49:5:</a> PLW0101 Code after return is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_baseoperator.py#L1023'>tests/models/test_baseoperator.py:1023:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L1144'>tests/models/test_mappedoperator.py:1144:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L1161'>tests/models/test_mappedoperator.py:1161:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L1463'>tests/models/test_mappedoperator.py:1463:17:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L1501'>tests/models/test_mappedoperator.py:1501:25:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L1548'>tests/models/test_mappedoperator.py:1548:25:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L858'>tests/models/test_mappedoperator.py:858:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L929'>tests/models/test_mappedoperator.py:929:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L945'>tests/models/test_mappedoperator.py:945:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_mappedoperator.py#L971'>tests/models/test_mappedoperator.py:971:21:</a> PLW0101 Code after raise is unreachable
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/tests/models/test_taskinstance.py#L2640'>tests/models/test_taskinstance.py:2640:13:</a> PLW0101 Code after raise is unreachable
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 14 | 14 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @diceroll123 on 2024-01-22 09:09_

---

_Comment by @diceroll123 on 2024-01-22 09:10_

Not sure what's causing this possible range error 🤔 

EDIT: skill issue, nvm

---

_Marked ready for review by @diceroll123 on 2024-01-23 04:05_

---

_Comment by @charliermarsh on 2024-01-24 18:41_

I'd prefer to support this by restoring and finishing our control-flow graph implementation: https://github.com/astral-sh/ruff/pull/5384. I think that abstraction will be more robust, and will also be useable in other parts of Ruff. Are you interested in looking into it?

---

_Comment by @diceroll123 on 2024-01-24 23:08_

oo, that's a big one, I'll possibly give it a whack sometime. For now I'll make this PR a draft.

---

_Converted to draft by @diceroll123 on 2024-01-24 23:09_

---

_Comment by @MichaReiser on 2024-03-28 17:30_

I close this PR as there hasn't been any progress in a while, and I agree with @charliermarsh that we need to build out a more holistic infrastructure for this.

Feel free to re-open it if you plan to work on this. 

---

_Closed by @MichaReiser on 2024-03-28 17:30_

---

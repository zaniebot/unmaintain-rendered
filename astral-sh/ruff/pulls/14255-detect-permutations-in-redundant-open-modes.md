```yaml
number: 14255
title: Detect permutations in redundant open modes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/redundant-open-mode
created_at: 2024-11-11T00:46:32Z
updated_at: 2024-11-11T03:48:32Z
url: https://github.com/astral-sh/ruff/pull/14255
synced_at: 2026-01-12T15:55:47Z
```

# Detect permutations in redundant open modes

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/14235.


---

_Label `bug` added by @charliermarsh on 2024-11-11 00:46_

---

_Comment by @dscorbett on 2024-11-11 00:57_

I think this implementation can change runtime behavior. If the mode string is invalid, like `t` or `UU` or `r?`, this new fix can make it valid, whereas it previously would raise a `ValueError`.

---

_Comment by @github-actions[bot] on 2024-11-11 01:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf3f9c7640a5f00af0a5117b146f32994daa6fe7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> UP015 [*] Unnecessary open mode parameters, use "a+"
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP015 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf3f9c7640a5f00af0a5117b146f32994daa6fe7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> UP015 [*] Unnecessary open mode parameters, use "a+"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP015 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-11-11 02:18_

Should we just ignore invalid codes entirely? There's a separate rule for them.

---

_Comment by @dscorbett on 2024-11-11 02:56_

Yes, I think UP015 should detect invalid modes as invalid and ignore them.

---

_Merged by @charliermarsh on 2024-11-11 03:48_

---

_Closed by @charliermarsh on 2024-11-11 03:48_

---

_Branch deleted on 2024-11-11 03:48_

---

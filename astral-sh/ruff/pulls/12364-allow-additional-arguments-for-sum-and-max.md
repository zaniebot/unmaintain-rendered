```yaml
number: 12364
title: Allow additional arguments for sum and max comprehensions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/C419
created_at: 2024-07-17T14:10:38Z
updated_at: 2024-07-18T12:37:30Z
url: https://github.com/astral-sh/ruff/pull/12364
synced_at: 2026-01-10T21:47:02Z
```

# Allow additional arguments for sum and max comprehensions

---

_Pull request opened by @charliermarsh on 2024-07-17 14:10_

## Summary

These can have other arguments, so it seems wrong to gate on single argument here.

Closes https://github.com/astral-sh/ruff/issues/12358.


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-17 14:10_

---

_@charliermarsh reviewed on 2024-07-17 14:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:943 on 2024-07-17 14:11_

Without this, we were (1) dropping commas in some tests (see fixture changes), and (2) adding an extra space after the comma when fixing `sum([x.val for x in bar], 0)` (not sure why?).

---

_Comment by @github-actions[bot] on 2024-07-17 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L47'>examples/models/calendars.py:47:31:</a> C419 Unnecessary list comprehension
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/86e2a915b15f47c65e5dfc3f19ccefb6646d687a/tests/store/tracking/__init__.py#L62'>tests/store/tracking/__init__.py:62:27:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/mlflow/mlflow/blob/86e2a915b15f47c65e5dfc3f19ccefb6646d687a/tests/store/tracking/test_file_store.py#L372'>tests/store/tracking/test_file_store.py:372:23:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/mlflow/mlflow/blob/86e2a915b15f47c65e5dfc3f19ccefb6646d687a/tests/store/tracking/test_sqlalchemy_store.py#L2548'>tests/store/tracking/test_sqlalchemy_store.py:2548:28:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/mlflow/mlflow/blob/86e2a915b15f47c65e5dfc3f19ccefb6646d687a/tests/store/tracking/test_sqlalchemy_store.py#L2562'>tests/store/tracking/test_sqlalchemy_store.py:2562:28:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/mlflow/mlflow/blob/86e2a915b15f47c65e5dfc3f19ccefb6646d687a/tests/store/tracking/test_sqlalchemy_store.py#L282'>tests/store/tracking/test_sqlalchemy_store.py:282:23:</a> C419 Unnecessary list comprehension
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C419 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @charliermarsh on 2024-07-17 15:38_

---

_@dhruvmanila reviewed on 2024-07-17 16:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:943 on 2024-07-17 16:58_

Hmm, I don't remember why this was None but this seems correct to me. It might've been some weird comment placement, not sure though.

---

_@dhruvmanila approved on 2024-07-17 16:58_

Good find!

---

_@AlexWaygood approved on 2024-07-18 12:24_

---

_Merged by @charliermarsh on 2024-07-18 12:37_

---

_Closed by @charliermarsh on 2024-07-18 12:37_

---

_Branch deleted on 2024-07-18 12:37_

---

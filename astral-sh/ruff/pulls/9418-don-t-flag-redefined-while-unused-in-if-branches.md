```yaml
number: 9418
title: "Don't flag `redefined-while-unused` in if branches"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/same-branch
created_at: 2024-01-07T01:40:57Z
updated_at: 2024-01-08T22:06:56Z
url: https://github.com/astral-sh/ruff/pull/9418
synced_at: 2026-01-10T22:57:09Z
```

# Don't flag `redefined-while-unused` in if branches

---

_Pull request opened by @charliermarsh on 2024-01-07 01:40_

## Summary

On `main`, we flag redefinitions in cases like:

```python
import os

x = 1

if x > 0:
    import os
```

That is, we consider these to be in the "same branch", since they're not in disjoint branches. This matches Flake8's behavior, but it seems to lead to false positives.


---

_Renamed from "Don't flag redefined-while-unsed in if branches" to "Don't flag `redefined-while-unsed` in if branches" by @charliermarsh on 2024-01-07 01:41_

---

_Comment by @charliermarsh on 2024-01-07 01:45_

Just looking at `# noqa: F811` usages in Code Search... At present, we often flag redefinitions within conditionals, and I don't see why we should:

```python
with safe_import() as exc:
    import cffi
if exc.error:
    cffi = None  # noqa: F811
```

```python
from kivy.core.window import Window
from kivy.metrics import dp
from kivy.utils import platform

if "KIVY_DOC_INCLUDE" in os.environ:
    dp = lambda x: x  # NOQA: F811
```

---

_Comment by @charliermarsh on 2024-01-07 01:51_

Enforcing this is also the source of the bug we introduced in https://github.com/astral-sh/ruff/issues/2044.

---

_Comment by @github-actions[bot] on 2024-01-07 01:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ef1498831a54c76a928d0a56eaf4c232925c0c65/tests/www/views/test_views_rendered.py#L259'>tests/www/views/test_views_rendered.py:259:55:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/util.py#L143'>src/bokeh/embed/util.py:143:32:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ef1498831a54c76a928d0a56eaf4c232925c0c65/tests/www/views/test_views_rendered.py#L259'>tests/www/views/test_views_rendered.py:259:55:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/util.py#L143'>src/bokeh/embed/util.py:143:32:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @zanieb by @charliermarsh on 2024-01-07 01:55_

---

_Renamed from "Don't flag `redefined-while-unsed` in if branches" to "Don't flag `redefined-while-unused` in if branches" by @charliermarsh on 2024-01-07 01:56_

---

_Label `rule` added by @charliermarsh on 2024-01-07 01:59_

---

_Comment by @charliermarsh on 2024-01-07 02:00_

Enforcing this would also resolve the bug introduced in https://github.com/astral-sh/ruff/issues/5561#issuecomment-1623720408.

---

_Comment by @charliermarsh on 2024-01-07 17:37_

Perhaps this should be preview though.

---

_@zanieb approved on 2024-01-08 21:57_

I think this is okay without preview since it's a reduction in scope; unless you think it is possible we will revert.

---

_Merged by @charliermarsh on 2024-01-08 22:06_

---

_Closed by @charliermarsh on 2024-01-08 22:06_

---

_Branch deleted on 2024-01-08 22:06_

---

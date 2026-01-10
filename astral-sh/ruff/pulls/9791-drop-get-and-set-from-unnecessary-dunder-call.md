```yaml
number: 9791
title: "Drop `__get__` and `__set__` from `unnecessary-dunder-call`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-get-set-dunder
created_at: 2024-02-02T17:11:37Z
updated_at: 2024-02-05T15:54:29Z
url: https://github.com/astral-sh/ruff/pull/9791
synced_at: 2026-01-10T22:57:09Z
```

# Drop `__get__` and `__set__` from `unnecessary-dunder-call`

---

_Pull request opened by @zanieb on 2024-02-02 17:11_

These are for descriptors which affects the behavior of the object _as a property_; I do not think they should be called directly but there is no alternative when working with the object directly.

Closes https://github.com/astral-sh/ruff/issues/9789


---

_Converted to draft by @zanieb on 2024-02-02 17:13_

---

_@charliermarsh approved on 2024-02-02 17:17_

Agreed

---

_Comment by @github-actions[bot] on 2024-02-02 17:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -7 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7932958488761cd01cd94b7eea0c646dc41c3981/airflow/serialization/serialized_objects.py#L875'>airflow/serialization/serialized_objects.py:875:17:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L394'>src/bokeh/core/property/descriptors.py:394:16:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L444'>src/bokeh/core/property/descriptors.py:444:21:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L652'>src/bokeh/core/property/descriptors.py:652:17:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L944'>src/bokeh/core/property/descriptors.py:944:17:</a> PLC2801 Unnecessary dunder call to `__set__`. Use subscript assignment.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_descriptors.py#L105'>tests/unit/bokeh/core/property/test_descriptors.py:105:13:</a> PLC2801 Unnecessary dunder call to `__set__`. Use subscript assignment.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_descriptors.py#L98'>tests/unit/bokeh/core/property/test_descriptors.py:98:13:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2801 | 7 | 0 | 7 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-02-02 17:27_

Unclear if we should drop the violation entirely. I think we should though — will require a bit more work and some test coverage.

---

_Comment by @charliermarsh on 2024-02-02 17:27_

Yeah I say we drop it.

---

_Comment by @AlexWaygood on 2024-02-02 17:45_

The suggestion for `__delete__` is similarly weird — calling `.__delete__()` on a descriptor instance is very different to using the `del` statement 

---

_Comment by @zanieb on 2024-02-02 18:31_

Ah good catch yeah that one is for descriptors too...

---

_Marked ready for review by @charliermarsh on 2024-02-05 15:33_

---

_Label `bug` added by @charliermarsh on 2024-02-05 15:33_

---

_@zanieb reviewed on 2024-02-05 15:38_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_dunder_call.py`:36 on 2024-02-05 15:38_

?

```suggestion
        item.__delete__(self)  # OK
```

---

_Comment by @zanieb on 2024-02-05 15:39_

Can't approve but lgtm, thanks!

---

_Merged by @charliermarsh on 2024-02-05 15:54_

---

_Closed by @charliermarsh on 2024-02-05 15:54_

---

_Branch deleted on 2024-02-05 15:54_

---

```yaml
number: 9581
title: Bump version to v0.1.14
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2024-01-19T17:03:43Z
updated_at: 2024-01-19T18:00:51Z
url: https://github.com/astral-sh/ruff/pull/9581
synced_at: 2026-01-12T15:55:29Z
```

# Bump version to v0.1.14

---

_@charliermarsh_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2024-01-19 17:09_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-19 17:09_

---

_Label `release` added by @charliermarsh on 2024-01-19 17:09_

---

_@zanieb approved on 2024-01-19 17:30_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2024-01-19 17:30_

```suggestion
- \[`flake8-pyi`\] Fix `PYI049` false negatives on call-based `TypedDict`s ([#9567](https://github.com/astral-sh/ruff/pull/9567))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2024-01-19 17:31_

```suggestion
- \[`pylint`\] Exclude `self` and `cls` when counting method arguments (`PLR0917`) ([#9563](https://github.com/astral-sh/ruff/pull/9563))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2024-01-19 17:32_

is this a bugfix or a performance feature?

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:40 on 2024-01-19 17:33_

```suggestion
- \[`refurb`\] Avoid bailing when `reimplemented-operator` is called on function (`FURB118`) ([#9556](https://github.com/astral-sh/ruff/pull/9556))
```

---

_@AlexWaygood reviewed on 2024-01-19 17:34_

LG, a few small suggestions:

---

_@AlexWaygood approved on 2024-01-19 17:45_

---

_Comment by @charliermarsh on 2024-01-19 17:45_

Thx!

---

_Merged by @charliermarsh on 2024-01-19 17:54_

---

_Closed by @charliermarsh on 2024-01-19 17:54_

---

_Branch deleted on 2024-01-19 17:54_

---

_Comment by @github-actions[bot] on 2024-01-19 18:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Required version `0.1.13` does not match the running version `0.1.14`
```

</p>
</details>




---

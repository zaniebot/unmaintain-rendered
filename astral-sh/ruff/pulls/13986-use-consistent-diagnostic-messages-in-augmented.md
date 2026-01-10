```yaml
number: 13986
title: Use consistent diagnostic messages in augmented assignment inference
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/diag
created_at: 2024-10-30T02:28:37Z
updated_at: 2024-10-30T12:24:44Z
url: https://github.com/astral-sh/ruff/pull/13986
synced_at: 2026-01-10T20:59:37Z
```

# Use consistent diagnostic messages in augmented assignment inference

---

_Pull request opened by @charliermarsh on 2024-10-30 02:28_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2024-10-30 02:28_

---

_Review requested from @carljm by @charliermarsh on 2024-10-30 02:28_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-30 02:28_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-30 02:28_

---

_@dhruvmanila approved on 2024-10-30 02:50_

---

_Comment by @github-actions[bot] on 2024-10-30 02:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>




---

_Merged by @charliermarsh on 2024-10-30 02:57_

---

_Closed by @charliermarsh on 2024-10-30 02:57_

---

_Branch deleted on 2024-10-30 02:57_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-30 12:24_

---

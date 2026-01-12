```yaml
number: 13985
title: Add remaining augmented assignment dunders
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/iadd
created_at: 2024-10-30T02:27:49Z
updated_at: 2024-11-01T13:31:33Z
url: https://github.com/astral-sh/ruff/pull/13985
synced_at: 2026-01-12T15:55:46Z
```

# Add remaining augmented assignment dunders

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/ruff/issues/12699


---

_Marked ready for review by @charliermarsh on 2024-10-30 02:29_

---

_Review requested from @carljm by @charliermarsh on 2024-10-30 02:29_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-30 02:29_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-30 02:29_

---

_Converted to draft by @charliermarsh on 2024-10-30 02:41_

---

_Comment by @github-actions[bot] on 2024-10-30 02:56_

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

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

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

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
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

_Marked ready for review by @charliermarsh on 2024-10-30 02:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1443 on 2024-10-30 08:16_

I would probably add another method to the `Operator` enum itself, similar to the existing ones; this could be generally useful for other users of the AST as well:

https://github.com/astral-sh/ruff/blob/1607d88c2253473a116155176effa4ab14eafcce/crates/ruff_python_ast/src/nodes.rs#L2975-L3008



---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1430 on 2024-10-30 08:17_

you could maybe take care of my post-merge review comment https://github.com/astral-sh/ruff/pull/13981#discussion_r1822075752 in this PR as well

---

_@AlexWaygood approved on 2024-10-30 08:22_

---

_Merged by @charliermarsh on 2024-10-30 13:02_

---

_Closed by @charliermarsh on 2024-10-30 13:02_

---

_Branch deleted on 2024-10-30 13:02_

---

_Label `red-knot` added by @dhruvmanila on 2024-11-01 13:31_

---

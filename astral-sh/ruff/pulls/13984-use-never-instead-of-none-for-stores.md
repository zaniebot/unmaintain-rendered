```yaml
number: 13984
title: "Use `Never` instead of `None` for stores"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/never
created_at: 2024-10-30T02:16:46Z
updated_at: 2024-11-01T13:31:41Z
url: https://github.com/astral-sh/ruff/pull/13984
synced_at: 2026-01-10T20:59:37Z
```

# Use `Never` instead of `None` for stores

---

_Pull request opened by @charliermarsh on 2024-10-30 02:16_

## Summary

See: https://github.com/astral-sh/ruff/pull/13981#issuecomment-2445472433


---

_Marked ready for review by @charliermarsh on 2024-10-30 02:18_

---

_Review requested from @carljm by @charliermarsh on 2024-10-30 02:18_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-30 02:18_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-30 02:18_

---

_@charliermarsh reviewed on 2024-10-30 02:18_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:95 on 2024-10-30 02:18_

I'm having trouble tracing this -- where is this diagnostic even coming from today?

---

_Comment by @github-actions[bot] on 2024-10-30 02:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
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

_@dhruvmanila reviewed on 2024-10-30 02:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:95 on 2024-10-30 02:36_

What's the issue? Is this failing? I think that's probably coming from `infer_starred_expression`

---

_@charliermarsh reviewed on 2024-10-30 02:42_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:95 on 2024-10-30 02:42_

The diagnostic is no longer showing up, so I had to remove these assertions along with the TODOs to remove them (?).

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:95 on 2024-10-30 02:49_

Yeah, I think that's correct. The diagnostics shouldn't be present in these cases and I think I left out one TODO in this specific case which might've confused you (?). The reason these diagnostics were showing up in the first place is because red knot tries to infer the starred expression (`*b`) but that isn't supported yet, before this PR it would've returned `None` and then it would raise a "not-an-iterable" diagnostic. But, it's now changed to use `Never` in this PR.

https://github.com/astral-sh/ruff/blob/c6b82151dddf842039aad72051a942c47e0759dc/crates/red_knot_python_semantic/src/types/infer.rs#L2341-L2344

---

_@dhruvmanila reviewed on 2024-10-30 02:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:95 on 2024-10-30 15:52_

This PR shouldn't even affect this case (we should never use the type of a Store-context expression), but it does just because we don't support assigning to Starred yet, and the way the temporary code works out, it does currently try to iterate the inferred type of a Store-context expression. But not throwing the error is better, so if anything switching from `None` to `Never` is an improvement here.

---

_@carljm approved on 2024-10-30 15:53_

---

_Merged by @charliermarsh on 2024-10-30 16:03_

---

_Closed by @charliermarsh on 2024-10-30 16:03_

---

_Branch deleted on 2024-10-30 16:03_

---

_Label `red-knot` added by @dhruvmanila on 2024-11-01 13:31_

---

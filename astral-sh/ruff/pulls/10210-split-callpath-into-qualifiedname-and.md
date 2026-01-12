```yaml
number: 10210
title: "Split `CallPath` into `QualifiedName` and `UnqualifiedName`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rename-call-path
created_at: 2024-03-03T16:25:46Z
updated_at: 2024-03-04T09:18:07Z
url: https://github.com/astral-sh/ruff/pull/10210
synced_at: 2026-01-12T15:55:31Z
```

# Split `CallPath` into `QualifiedName` and `UnqualifiedName`

---

_@MichaReiser_

## Summary

Charlie can probably explain this better than I but it turns out, `CallPath` is used for two different things:

* To represent unqualified names like `version` where `version` can be a local variable or imported (e.g. `from sys import version` where the full qualified name is `sys.version`)
* To represent resolved, full qualified names

This PR splits `CallPath` into two types to make this destinction clear. 

> Note: I haven't renamed all `call_path` variables to `qualified_name` or `unqualified_name`. I can do that if that's welcomed but I first want to get feedback on the approach and naming overall.

## Test Plan

`cargo test`


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-03 16:25_

---

_Label `internal` added by @MichaReiser on 2024-03-03 16:26_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-03 16:27_

---

_Comment by @github-actions[bot] on 2024-03-03 16:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/_libs/reshape.pyi#L6'>pandas/_libs/reshape.pyi:6:5:</a> PLR0917 Too many positional arguments (7/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-03-03 20:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/name.rs`:141 on 2024-03-03 20:12_

(Do these need to be `pub`? Are there usages that don't go through the structs?)

---

_@charliermarsh approved on 2024-03-03 20:12_

---

_@MichaReiser reviewed on 2024-03-03 20:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:141 on 2024-03-03 20:14_

There are :( I can try to eliminate it entirely because it's really just a `UnqualifiedName::from_expr(expr).map(ToString::to_string)` and this doesm't seem a good pattern IMO

---

_Merged by @MichaReiser on 2024-03-04 09:06_

---

_Closed by @MichaReiser on 2024-03-04 09:06_

---

_Branch deleted on 2024-03-04 09:06_

---

```yaml
number: 11926
title: "[red-knot] Extract `red_knot_python_semantic` crate"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-9-red_knot_python_semantic
created_at: 2024-06-18T14:16:29Z
updated_at: 2024-06-20T11:38:22Z
url: https://github.com/astral-sh/ruff/pull/11926
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Extract `red_knot_python_semantic` crate

---

_Pull request opened by @MichaReiser on 2024-06-18 14:16_

## Summary

Extract the red knot code from `ruff_python_semantic` and move it into its own crate because we don't expect to share any logic between the two semantic models.

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2024-06-18 14:16_

---

_Label `red-knot` added by @MichaReiser on 2024-06-18 14:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-18 14:18_

---

_@AlexWaygood approved on 2024-06-18 14:21_

---

_Comment by @MichaReiser on 2024-06-18 15:46_

Oh, I missed some doctests. I'll fix the test failures before merging.

---

_@carljm approved on 2024-06-18 16:20_

---

_@MichaReiser reviewed on 2024-06-20 11:05_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:180 on 2024-06-20 11:05_

I removed this declaration and instead use the one from` red_knot_python_semantic` to silence `cargo shear` that the `red_knot_python_semantic` crate is unused (which is true, but not for long)

---

_Merged by @MichaReiser on 2024-06-20 11:24_

---

_Closed by @MichaReiser on 2024-06-20 11:24_

---

_Branch deleted on 2024-06-20 11:24_

---

_Comment by @github-actions[bot] on 2024-06-20 11:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/704fa5d1decd72857d9b22378abfcf4bfbc00bdd/stdlib/dis.pyi#L144'>stdlib/dis.pyi:144:1:</a> E999 SyntaxError: unexpected EOF while parsing
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

```yaml
number: 15081
title: "Support file-level `type: ignore` comments"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/file-level-type-ignore
created_at: 2024-12-20T12:59:32Z
updated_at: 2024-12-23T10:01:14Z
url: https://github.com/astral-sh/ruff/pull/15081
synced_at: 2026-01-12T15:55:50Z
```

# Support file-level `type: ignore` comments

---

_@MichaReiser_

## Summary

This PR adds support for file-level `type: ignore` comments

> A `# type: ignore` comment on a line by itself at the top of a file, before any docstrings, imports, or other executable code, silences all errors in the file. Blank lines and other comments, such as shebang lines and coding cookies, may precede the `# type: ignore` comment. [source](https://typing.readthedocs.io/en/latest/spec/directives.html#type-ignore-comments)

I intentionally didn't enable this behavior for `knot: ignore` comments because we may want to introduce a distinct syntax that not only allows suppressing errors but also enabling rules or changing the rule's severity: `knot: possibly-undefined-variable="warn"`

## Test Plan

Added mtests


---

_Label `red-knot` added by @MichaReiser on 2024-12-20 12:59_

---

_@MichaReiser reviewed on 2024-12-20 13:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:198 on 2024-12-20 13:00_

This is the only new code in this block. Everything else was extracted from the `suppressions` functions

---

_Marked ready for review by @MichaReiser on 2024-12-20 13:01_

---

_Review requested from @carljm by @MichaReiser on 2024-12-20 13:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-20 13:01_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-20 13:01_

---

_Comment by @github-actions[bot] on 2024-12-20 13:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:153 on 2024-12-22 16:24_

Should this emit a diagnostic? Or not necessary, because it just won't work and that will be obvious from other diagnostics in the file?

---

_@carljm approved on 2024-12-22 16:27_

---

_@MichaReiser reviewed on 2024-12-23 09:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:153 on 2024-12-23 09:55_

This gets flagged by the `unused-ignore-comment` rule


https://github.com/astral-sh/ruff/blob/micha/unused-ignore-comment/crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md#invalid-own-line-suppression

---

_Merged by @MichaReiser on 2024-12-23 09:59_

---

_Closed by @MichaReiser on 2024-12-23 09:59_

---

_Branch deleted on 2024-12-23 09:59_

---

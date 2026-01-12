```yaml
number: 13059
title: "Move collection of parse errors to `check_file`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: add-parse-errors-in-check-file
created_at: 2024-08-22T15:47:33Z
updated_at: 2024-08-23T06:22:14Z
url: https://github.com/astral-sh/ruff/pull/13059
synced_at: 2026-01-12T15:55:43Z
```

# Move collection of parse errors to `check_file`

---

_@MichaReiser_

## Summary

We should unifiy the diagnostic serialization but I leave this for when we add proper diagnostic structs (and reporter)

Fixes #13058

## Test Plan

I added a syntax error to a file

```
ERROR Method Test.foo is decorated with `typing.override` but does not override any base class method
ERROR /home/micha/astral/test/x.py:1:74: Expected an indented block after function definition
```


---

_Review requested from @carljm by @MichaReiser on 2024-08-22 15:47_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-22 15:47_

---

_Label `red-knot` added by @MichaReiser on 2024-08-22 15:47_

---

_Comment by @github-actions[bot] on 2024-08-22 16:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-08-22 21:09_

Thank you!

---

_Merged by @MichaReiser on 2024-08-23 06:22_

---

_Closed by @MichaReiser on 2024-08-23 06:22_

---

_Branch deleted on 2024-08-23 06:22_

---

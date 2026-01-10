```yaml
number: 14201
title: "[red-knot] Use `return_ref` for `Class::try_metaclass`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/avoid-metaclass-clone
created_at: 2024-11-08T12:36:46Z
updated_at: 2024-11-08T12:55:24Z
url: https://github.com/astral-sh/ruff/pull/14201
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Use `return_ref` for `Class::try_metaclass`

---

_Pull request opened by @AlexWaygood on 2024-11-08 12:36_

## Summary

`Type` is cheap to clone, but `MetaclassError` is potentially quite expensive. Use `return_ref` to return a reference to the cached data rather than cloning it every time the method is called.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-08 12:36_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-08 12:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-08 12:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-08 12:36_

---

_Comment by @github-actions[bot] on 2024-11-08 12:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-08 12:52_

I would keep it as is: `MetaClassError`s are rare. That's why most `Result` clones are as cheap: they simply clone  a `Type`. `MetaclassError` itself contains no heap allocated data, making it also relatively cheap to clone. 

---

_Comment by @AlexWaygood on 2024-11-08 12:54_

Thanks, that makes sense. I guess it's pretty different to `try_mro`, where we allocate a boxed slice in all cases, even in the happy path. (There are definitely more optimisations we can take there if it shows up in profiles, FWIW.)

---

_Closed by @AlexWaygood on 2024-11-08 12:54_

---

_Branch deleted on 2024-11-08 12:54_

---

```yaml
number: 16038
title: "[`pyupgrade`] Ensure we do not rename two type parameters to the same name (`UP049`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: alex/up049-multiple
created_at: 2025-02-08T12:05:11Z
updated_at: 2025-02-08T15:44:06Z
url: https://github.com/astral-sh/ruff/pull/16038
synced_at: 2026-01-10T19:57:22Z
```

# [`pyupgrade`] Ensure we do not rename two type parameters to the same name (`UP049`)

---

_Pull request opened by @AlexWaygood on 2025-02-08 12:05_

Fixes #16024

## Summary

This PR adds proper isolation for `UP049` fixes so that two type parameters are not renamed to the same name, which would introduce invalid syntax. E.g. for this:

```py
class Foo[_T, __T]: ...
```

we cannot apply two autofixes to the class, as that would produce invalid syntax -- this:

```py
class Foo[T, T]: ...
```

The "isolation" here means that Ruff won't apply more than one fix to the same type-parameter list in a single iteration of the loop it does to apply all autofixes. This means that after the first autofix has been done, the semantic model will have recalculated which variables are available in the scope, meaning that the diagnostic for the second parameter will be deemed unfixable since it collides with an existing name in the same scope (the name we autofixed the first parameter to in an earlier iteration of the autofix loop).

Cc. @ntBre, for interest!

## Test Plan

I added an integration test that reproduces the bug on `main`.


---

_Label `bug` added by @AlexWaygood on 2025-02-08 12:05_

---

_Label `fixes` added by @AlexWaygood on 2025-02-08 12:05_

---

_Comment by @github-actions[bot] on 2025-02-08 12:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-02-08 15:40_

LGTM! And good to know about `isolation`, thanks!

---

_Merged by @AlexWaygood on 2025-02-08 15:44_

---

_Closed by @AlexWaygood on 2025-02-08 15:44_

---

_Branch deleted on 2025-02-08 15:44_

---

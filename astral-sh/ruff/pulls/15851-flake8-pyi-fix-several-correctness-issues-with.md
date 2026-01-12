```yaml
number: 15851
title: "[`flake8-pyi`] Fix several correctness issues with `custom-type-var-return-type` (`PYI019`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: alex/pyi-panic
created_at: 2025-01-31T13:34:03Z
updated_at: 2025-01-31T14:21:37Z
url: https://github.com/astral-sh/ruff/pull/15851
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-pyi`] Fix several correctness issues with `custom-type-var-return-type` (`PYI019`)

---

_@AlexWaygood_

## Summary

This PR fixes several correctness issues in our `PYI019` rule:
- The rule would panic on this snippet. There shouldn't even be a diagnostic on this snippet, since it's not a valid use of the `S` type variable:
  ```py
  class F:
      @classmethod
      def m[S](cls: type[S], /) -> S[int]: ...
  ```
- The rule would panic on this snippet. Again, there shouldn't even be a diagnostic emitted here; it's an invalid use of a type variable:
  ```py
  class F:
      def m[S](self: S) -> S[int]: ...
  ```
- The rule had a false positive on this snippet, where `type` is shadowed locally:
  ```py
  def shadowed_type():
      type = 1
      class A:
          @classmethod
          def m[S](cls: type[S]) -> S: ...
  ```
- The rule had a false negative on this snippet, where the fully qualified version of `builtins.type` rather than the implicit builtin binding:
  ```py
  import builtins

  class UsesFullyQualifiedType:
      @classmethod
      def m[S](cls: builtins.type[S]) -> S: ...  # PYI019
  ```
- The rule had a false negative on this snippet, where the return annotation is `type[S]` rather than simply `S`:
  ```py
  class Foo:
      @classmethod
      def f[S](cls: type[S]) -> type[S]: ...
  ```

Fixes #15849

## Test Plan

I've added new fixtures added that trigger panics and/or incorrect behaviour on `main`.


---

_Label `bug` added by @AlexWaygood on 2025-01-31 13:34_

---

_Label `rule` added by @AlexWaygood on 2025-01-31 13:34_

---

_Review requested from @ntBre by @AlexWaygood on 2025-01-31 13:35_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-31 13:35_

---

_Comment by @github-actions[bot] on 2025-01-31 13:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:7 on 2025-01-31 14:10_

nit: do you want to combine these?

---

_@ntBre approved on 2025-01-31 14:12_

This looks great! I had to throw in one nit, but the changes were easy to follow, especially with the new variable names, and the tests made it clear what was going on.

---

_@AlexWaygood reviewed on 2025-01-31 14:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:7 on 2025-01-31 14:12_

ohh, thanks!

---

_Merged by @AlexWaygood on 2025-01-31 14:19_

---

_Closed by @AlexWaygood on 2025-01-31 14:19_

---

_Branch deleted on 2025-01-31 14:19_

---

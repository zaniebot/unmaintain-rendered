```yaml
number: 22625
title: "[ty] Correct return type for synthesized NamedTuple.__new__ methods"
type: pull_request
state: open
author: bxff
labels: []
assignees: []
base: main
head: fix-namedtuple-new
created_at: 2026-01-16T17:58:13Z
updated_at: 2026-01-16T17:58:53Z
url: https://github.com/astral-sh/ruff/pull/22625
synced_at: 2026-01-16T18:01:01Z
```

# [ty] Correct return type for synthesized NamedTuple.__new__ methods

---

_@bxff_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixed the synthesized `NamedTuple.__new__` method to correctly return `Self` instead of the base class literal. This resolves `invalid-return-type` errors in subclasses that override `__new__` and call `super().__new__()`.

The change introduces a synthetic `Self` type variable for the `__new__` signature, making it properly generic. The `cls` parameter now uses `type[Self]` and the return type is `Self`, matching Python's runtime behavior and typing conventions.

Fixes https://github.com/astral-sh/ty/issues/2522

## Test Plan

- Added regression test to `crates/ty_python_semantic/resources/mdtest/named_tuple.md` verifying:
  - `reveal_type(instance)` inside `Child.__new__` shows `Self@__new__`
  - `reveal_type(Child(1, 2))` correctly shows `Child`
  
- Updated existing assertions in `named_tuple.md` to reflect the new generic signature:
  ```
  reveal_type(Url.__new__)  # revealed: [Self](cls: type[Self], protocol: str, host: str, port: int | None = ...) -> Self
  ```

---

_Review requested from @carljm by @bxff on 2026-01-16 17:58_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-16 17:58_

---

_Review requested from @sharkdp by @bxff on 2026-01-16 17:58_

---

_Review requested from @dcreager by @bxff on 2026-01-16 17:58_

---

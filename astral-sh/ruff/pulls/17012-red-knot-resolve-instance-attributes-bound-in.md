```yaml
number: 17012
title: "[red-knot] resolve instance attributes bound in nested scopes"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
base: main
head: 17008-nested-scope-attribute-assignment
created_at: 2025-03-27T15:13:59Z
updated_at: 2025-03-30T05:57:22Z
url: https://github.com/astral-sh/ruff/pull/17012
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] resolve instance attributes bound in nested scopes

---

_Pull request opened by @mtshiba on 2025-03-27 15:13_

## Summary

This PR closes astral-sh/ty#147.

Instance attributes bound in nested scopes are now accessible from the instance.

## Test Plan

New test cases are added to `mdtest/attributes.md`, and [this test case](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/attributes.md#attributes-defined-in-comprehensions) now passes correctly.

---

_Review requested from @carljm by @mtshiba on 2025-03-27 15:14_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-27 15:14_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-27 15:14_

---

_Review requested from @dcreager by @mtshiba on 2025-03-27 15:14_

---

_Comment by @github-actions[bot] on 2025-03-27 15:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-03-27 16:02_

---

_Comment by @MichaReiser on 2025-03-27 16:04_

This seems like a good one for @sharkdp 

---

_Assigned to @sharkdp by @MichaReiser on 2025-03-27 16:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:157 on 2025-03-28 15:23_

Maybe
```suggestion
    fn class_scope_of_method(&self, attribute_access_target: Option<&str>) -> Option<FileScopeId> {
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:588 on 2025-03-28 15:23_

Similar here: maybe
```suggestion
        let attribute_access_target = object.as_name_expr().map(|name| name.id().as_str());
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:156 on 2025-03-28 15:24_

```suggestion
    /// function body (or nested within a function body).
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:168 on 2025-03-28 15:31_

```suggestion
                    // The current scope is a function (method) in which the attribute assignment appeared.
```

---

_Comment by @carljm on 2025-03-28 16:47_

Haven't looked at the code, just a general comment that I think it's not clear we need to support this at all. Mypy does, but pyright does not. I'm not _opposed_ to supporting it, but I don't think it's a priority, and I wouldn't want to spend a lot of time on it if it is tricky.

---

_@sharkdp reviewed on 2025-03-28 17:36_

I discussed these changes with @AlexWaygood and we think that this approach is problematic in some cases. Consider for example:
```py
class Unrelated:
    x: int

class C:
    def __init__(self):
        def g():
            self = Unrelated()
            self.x = 42

reveal_type(C().x)
```
where we emit `Unrelated | Literal[42]` at the moment, but `C` instances don't have an `x` attribute. Similarly, if you have something like
```py
class C:
    def __init__(self):
        def g(this):
            this.x = 42

        g(self)

reveal_type(C().x)
```
we would not find that attribute assignment, even though it seems conceptually similar to the test case we are solving here. So I agree with @carljm that it seems questionable if we want to support this (now).

---

_Comment by @mtshiba on 2025-03-30 05:57_

I see. To confirm that `self` in `self.foo = ...` is actually the first argument of the method, it is not enough to check the first parameter of the method; we also need to check that `self` has not been overwritten before the attribute assignment.

To check this, we would need to call `UseDefMap::bindings_at_use` by using `self`'s `ScopedUseId`, I guess. However, this is not possible at the time when `SemanticIndexBuilder::register_attribute_assignment` is called, because the building of `UseDefMap` has not been completed.

Also, even if the parameter name is different, the method can actually write to `self` by receiving `self` as an argument.

Thus, trying to get this right would require some tricky handling... I certainly don't see much need to implement such a handling at this time.

---

_Closed by @mtshiba on 2025-03-30 05:57_

---

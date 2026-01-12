```yaml
number: 2085
title: "panic:  execute: too many cycle iterations"
type: issue
state: closed
author: ncollins-vs
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-12-18T19:30:14Z
updated_at: 2025-12-23T00:16:05Z
url: https://github.com/astral-sh/ty/issues/2085
synced_at: 2026-01-12T15:54:26Z
```

# panic:  execute: too many cycle iterations

---

_@ncollins-vs_

### Summary

I get a fatal error checking this code:

```python
class Test:
    def __init__(self, points: list[tuple[float, float] | None]) -> None:
        self.left = points
        self.right = points
    def method(self) -> None:
        self.left, self.right = self.right, self.left
        if self.right:
            self.right = [
                ((-point[0], point[1]) if point else point) for point in self.right
            ]
```

Error:
```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:321:21 when checking `<filename>`: `infer_definition_types(Id(1407)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.3 (fadfe0966 2025-12-17)
info: Args: ["ty", "check", "<filename>"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_expression_types_impl(Id(1803))
             at crates/ty_python_semantic/src/types/infer.rs:196
   1: static_expression_truthiness(Id(1803))
             at crates/ty_python_semantic/src/types/infer.rs:394
   2: infer_expression_types_impl(Id(1804))
             at crates/ty_python_semantic/src/types/infer.rs:196
   3: infer_expression_types_impl(Id(1806))
             at crates/ty_python_semantic/src/types/infer.rs:196
   4: infer_expression_type_impl(Id(1806))
             at crates/ty_python_semantic/src/types/infer.rs:273
   5: ClassLiteral < 'db >::implicit_attribute_inner_(Id(6c08))
             at crates/ty_python_semantic/src/types/class.rs:1524
             cycle heads: infer_unpack_types(Id(1c00)) -> iteration = 200
   6: Type < 'db >::member_lookup_with_policy_(Id(500e))
             at crates/ty_python_semantic/src/types.rs:881
   7: infer_expression_types_impl(Id(1802))
             at crates/ty_python_semantic/src/types/infer.rs:196
   8: infer_unpack_types(Id(1c00))
             at crates/ty_python_semantic/src/types/infer.rs:425
   9: ClassLiteral < 'db >::implicit_attribute_inner_(Id(6c07))
             at crates/ty_python_semantic/src/types/class.rs:1524
  10: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:69
  11: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:534


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

Oddly, the error doesn't happen for `left`, and it goes away if I use `if point else None`

https://play.ty.dev/5f817b19-ad82-4a44-9ec6-9d40e72b37f0

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Label `fatal` added by @carljm on 2025-12-18 19:54_

---

_Comment by @carljm on 2025-12-18 19:55_

Thanks for the report! Will take a look

---

_Added to milestone `Stable` by @carljm on 2025-12-18 19:55_

---

_Label `bug` added by @AlexWaygood on 2025-12-18 20:07_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-19 07:37_

---

_Comment by @mtshiba on 2025-12-19 07:51_

This appears to be the same kind of bug I found while working on astral-sh/ruff#17371 (https://github.com/astral-sh/ruff/pull/17371/commits/1d1afaa517836c1974ce4d780dcee47a7c258387).
I opened a separate PR astral-sh/ruff#22070.


---

_Closed by @carljm on 2025-12-23 00:16_

---

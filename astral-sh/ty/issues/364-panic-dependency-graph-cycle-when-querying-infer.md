```yaml
number: 364
title: "panic: dependency graph cycle when querying infer_unpack_types"
type: issue
state: closed
author: jelle-openai
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-05-13T18:27:12Z
updated_at: 2025-05-13T21:27:50Z
url: https://github.com/astral-sh/ty/issues/364
synced_at: 2026-01-12T15:54:23Z
```

# panic: dependency graph cycle when querying infer_unpack_types

---

_@jelle-openai_

### Summary

I got the following crash on an internal file:

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/fetch.rs:129:25 when checking `<snip>.py`: `dependency graph cycle when querying infer_unpack_types(Id(2efadb)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(fc70)),
    infer_scope_types(Id(c987b2)),
    member_lookup_with_policy_(Id(cd05be)),
    infer_unpack_types(Id(2efadb)),
    infer_expression_types(Id(4f5c4c)),
    member_lookup_with_policy_(Id(cd05c2)),
    infer_expression_type(Id(4f5c37)),
    infer_expression_types(Id(4f5c37)),
    all_narrowing_constraints_for_expression(Id(4f5c36)),
    infer_definition_types(Id(4f595e)),
    infer_expression_types(Id(4f5c73)),
    signature_(Id(cba6c1)),
    to_overloaded_(Id(cba6c1)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: member_lookup_with_policy_(Id(cd05c2))
             at crates/ty_python_semantic/src/types.rs:536
   1: infer_expression_types(Id(4f5c4c))
             at crates/ty_python_semantic/src/types/infer.rs:222
             cycle heads: member_lookup_with_policy_(Id(cd05be)) -> 0
   2: infer_unpack_types(Id(2efadb))
             at crates/ty_python_semantic/src/types/infer.rs:309
   3: member_lookup_with_policy_(Id(cd05be))
             at crates/ty_python_semantic/src/types.rs:536
   4: infer_scope_types(Id(c987b2))
             at crates/ty_python_semantic/src/types/infer.rs:121
   5: check_types(Id(fc70))
             at crates/ty_python_semantic/src/types.rs:84
```

I haven't tried to minimize it but can do so if helpful. 

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Label `bug` added by @AlexWaygood on 2025-05-13 18:31_

---

_Label `fatal` added by @AlexWaygood on 2025-05-13 18:31_

---

_Comment by @AlexWaygood on 2025-05-13 18:31_

> I haven't tried to minimize it but can do so if helpful.

yes please!

---

_Comment by @JelleZijlstra on 2025-05-13 18:57_

Unfortunately it depends on a third-party package.

Install `pymupdf` (which provides the `fitz` import), then run `ty check` on this:

```python
import fitz

def a():
    def b(rect: fitz.Rect, delta: int = 5):
        return fitz.Rect(rect.x0 - delta, rect.y0 - delta, rect.x1 + delta, rect.y1 * delta)
```



---

_Comment by @dhruvmanila on 2025-05-13 19:10_

Thank you! It seems like just accessing an attribute triggers the cycle:

```py
import fitz


fitz.Rect().x0
```

I will spend a couple of minutes trying to minimize it further to remove the third-party dependency.

---

_Comment by @dhruvmanila on 2025-05-13 19:39_

A minimal reproduction:

```py
class Foo:
    def method(self):
        other = Foo()
        self.x, self.y = other.x, other.y


Foo().x
```

With the trace logs where the cycle occurs:
```
2  └─┐ty_python_semantic::types::infer::infer_unpack_types{range=63..77, file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2    └─┐ty_python_semantic::types::infer::infer_expression_types{expression=Id(1401), range=80..96, file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2      └─┐ty_python_semantic::types::infer::infer_definition_types{range=41..46, file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2        └─┐ty_python_semantic::types::infer::infer_expression_types{expression=Id(1400), range=49..54, file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2          └─┐ty_python_semantic::semantic_index::global_scope{file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2          ┌─┘
2          └─┐ty_python_semantic::symbol::symbol{name="Foo"}
2            └─┐ty_python_semantic::semantic_index::symbol_table{scope=Id(c00), file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2            ┌─┘
2            └─┐ty_python_semantic::semantic_index::use_def_map{scope=Id(c00), file=File(System("/Users/dhruv/playground/ty/isolated1/play.py"))}
2            ┌─┘
2          ┌─┘
2          └─┐ty_python_semantic::symbol::symbol{name="Enum"}
2          ┌─┘
2        ┌─┘
2      ┌─┘
2      ├─   0.106922s   0ms TRACE ty_python_semantic::types member_lookup_with_policy: Foo.y
2    ┌─┘
2  ┌─┘
2┌─┘
2┘
fatal[panic] Panicked at /Users/dhruv/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/fetch.rs:129:25 when checking `/Users/dhruv/playground/ty/isolated1/play.py`: `dependency graph cycle when querying infer_unpack_types(Id(1800)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(800)),
    infer_scope_types(Id(c00)),
    member_lookup_with_policy_(Id(4007)),
    infer_unpack_types(Id(1800)),
    infer_expression_types(Id(1401)),
    member_lookup_with_policy_(Id(4008)),
    infer_expression_types(Id(1400)),
    symbol_by_id(Id(280b)),
    use_def_map(Id(c00)),
    symbol_by_id(Id(2809)),
    infer_definition_types(Id(69d6)),
    file_to_module(Id(2425)),
    member_lookup_with_policy_(Id(4006)),
    imported_modules(Id(2425)),
    member_lookup_with_policy_(Id(4003)),
    symbol_by_id(Id(2807)),
    infer_definition_types(Id(5453)),
    file_to_module(Id(241c)),
    source_text(Id(241c)),
]
```

The cycle is occurring because accessing `Foo().x` requires the `infer_unpack_types` on the unpacking assignment but inferring the right-hand side expression itself requires the `infer_unpack_types` on the same `Unpack` ingredient because `other` is also an instance of `Foo`.

---

_Comment by @carljm on 2025-05-13 19:48_

That looks like a legitimate cycle, so the right answer here should just be a PR to add fixpoint handling to `infer_unpack_types` query.

---

_Closed by @dhruvmanila on 2025-05-13 21:27_

---

```yaml
number: 1630
title: "[panic] infer_expression_type_impl(Id(5126d)): execute: too many cycle iterations"
type: issue
state: closed
author: cclauss
labels:
  - fatal
assignees: []
created_at: 2025-11-25T11:42:29Z
updated_at: 2025-11-25T15:34:41Z
url: https://github.com/astral-sh/ty/issues/1630
synced_at: 2026-01-12T15:54:25Z
```

# [panic] infer_expression_type_impl(Id(5126d)): execute: too many cycle iterations

---

_@cclauss_

Codebase: https://github.com/celery/kombu
Python file: https://github.com/celery/kombu/blob/main/kombu/resource.py

The panic disappears if `--exclude=kombu/resource.py` is added.
```
RUST_BACKTRACE=1 ty check \
  --ignore=call-non-callable \
  --ignore=conflicting-declarations \
  --ignore=invalid-argument-type \
  --ignore=invalid-assignment \
  --ignore=invalid-base \
  --ignore=invalid-exception-caught \
  --ignore=invalid-parameter-default \
  --ignore=invalid-return-type \
  --ignore=no-matching-overload \
  --ignore=non-subscriptable \
  --ignore=not-iterable \
  --ignore=parameter-already-assigned \
  --ignore=possibly-missing-attribute \
  --ignore=possibly-missing-import \
  --ignore=too-many-positional-arguments \
  --ignore=unresolved-attribute \
  --ignore=unresolved-import \
  --ignore=unresolved-reference \
  --ignore=unknown-argument \
  --ignore=unsupported-operator
```
```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `HOME/kombu/kombu/resource.py`: `infer_expression_type_impl(Id(5126d)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.27 (26d7b6864 2025-11-18)
info: Args: ["ty", "check", "--ignore=call-non-callable", "--ignore=conflicting-declarations", "--ignore=invalid-argument-type", "--ignore=invalid-assignment", "--ignore=invalid-base", "--ignore=invalid-exception-caught", "--ignore=invalid-parameter-default", "--ignore=invalid-return-type", "--ignore=no-matching-overload", "--ignore=non-subscriptable", "--ignore=not-iterable", "--ignore=parameter-already-assigned", "--ignore=possibly-missing-attribute", "--ignore=possibly-missing-import", "--ignore=too-many-positional-arguments", "--ignore=unresolved-attribute", "--ignore=unresolved-import", "--ignore=unresolved-reference", "--ignore=unknown-argument", "--ignore=unsupported-operator"]
info: Backtrace:
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __mh_execute_header
  30: __mh_execute_header
  31: __mh_execute_header
  32: __mh_execute_header
  33: __mh_execute_header
  34: __mh_execute_header
  35: __mh_execute_header
  36: __mh_execute_header
  37: __mh_execute_header
  38: __mh_execute_header
  39: __mh_execute_header
  40: __mh_execute_header
  41: __mh_execute_header
  42: __mh_execute_header
  43: __mh_execute_header
  44: __mh_execute_header
  45: __mh_execute_header
  46: __mh_execute_header
  47: __mh_execute_header
  48: __mh_execute_header
  49: __pthread_cond_wait

info: query stacktrace:
   0: ClassLiteral < 'db >::implicit_attribute_inner_(Id(3326a))
             at crates/ty_python_semantic/src/types/class.rs:1363
   1: Type < 'db >::member_lookup_with_policy_(Id(1629d))
             at crates/ty_python_semantic/src/types.rs:821
   2: infer_expression_types_impl(Id(5126d))
             at crates/ty_python_semantic/src/types/infer.rs:182
   3: infer_definition_types(Id(5572e))
             at crates/ty_python_semantic/src/types/infer.rs:94
   4: infer_scope_types(Id(32fc))
             at crates/ty_python_semantic/src/types/infer.rs:70
   5: check_file_impl(Id(141c))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Label `fatal` added by @AlexWaygood on 2025-11-25 11:53_

---

_Comment by @mtshiba on 2025-11-25 11:57_

MRE:

```python
class Foo:
    foo = 0
    def method(self):
        self.foo = self.foo + 1
```

This is a panic caused by recursive type inference, and will be fixed in astral-sh/ruff#20566.

---

_Closed by @carljm on 2025-11-25 15:34_

---

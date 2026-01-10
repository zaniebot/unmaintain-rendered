```yaml
number: 923
title: Recursive type panics
type: issue
state: closed
author: zdxerr
labels: []
assignees: []
created_at: 2025-08-01T09:02:02Z
updated_at: 2025-08-01T09:05:33Z
url: https://github.com/astral-sh/ty/issues/923
synced_at: 2026-01-10T02:06:24Z
```

# Recursive type panics

---

_Issue opened by @zdxerr on 2025-08-01 09:02_

### Summary

The type definition 

`type JSON = dict[str, JSON] | list[JSON] | str | int | float | bool | None`

panics:

```
[Error - 10:59:21 AM] Request textDocument/inlayHint failed.
  Message: request handler panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\dba66f1\src\function\fetch.rs:165:25:
dependency graph cycle when querying PEP695TypeAliasType < 'db >::value_type_(Id(b003g3)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    infer_scope_types(Id(1954cg1)),
    infer_definition_types(Id(19f71g1)),
    infer_scope_types(Id(1954b)),
    PEP695TypeAliasType < 'db >::value_type_(Id(b003g3)),
    infer_scope_types(Id(1007g1)),
]query stacktrace:
   0: DatabaseKeyIndex(IngredientIndex(81), Id(1007g1))
   1: DatabaseKeyIndex(IngredientIndex(80), Id(b003g3))
   2: DatabaseKeyIndex(IngredientIndex(81), Id(1954b))
   3: DatabaseKeyIndex(IngredientIndex(82), Id(19f71g1))
   4: DatabaseKeyIndex(IngredientIndex(81), Id(1954cg1))


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
```

### Version

0.0.1a16

---

_Closed by @AlexWaygood on 2025-08-01 09:05_

---

_Comment by @AlexWaygood on 2025-08-01 09:05_

Thanks for the report! Please see #256

---

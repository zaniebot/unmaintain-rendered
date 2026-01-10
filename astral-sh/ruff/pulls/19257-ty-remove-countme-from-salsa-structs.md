```yaml
number: 19257
title: "[ty] Remove countme from salsa-structs"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/remove-countme
created_at: 2025-07-10T11:41:43Z
updated_at: 2025-07-10T11:54:55Z
url: https://github.com/astral-sh/ruff/pull/19257
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Remove countme from salsa-structs

---

_Pull request opened by @MichaReiser on 2025-07-10 11:41_

## Summary

`TY_MEMORY_REPORT` gives us the same and maore information. Making the countme approach for salsa structs obsolete. 

E.g. `TY_MEMORY_REPORT` gives you

```
`Definition`                                       metadata=7.41MB   fields=17.77MB  count=185129
`Expression`                                       metadata=2.34MB   fields=3.12MB   count=48748
`OverloadLiteral`                                  metadata=1.48MB   fields=1.05MB   count=26361
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=0.78MB   fields=0.89MB   count=13874
`File`                                             metadata=0.81MB   fields=0.81MB   count=12712
`FunctionType`                                     metadata=1.51MB   fields=0.65MB   count=26956
`ScopeId`                                          metadata=1.39MB   fields=0.60MB   count=49716
`IntersectionType`                                 metadata=0.27MB   fields=0.55MB   count=4899
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=0.39MB   fields=0.44MB   count=6925
`FunctionLiteral`                                  metadata=1.48MB   fields=0.42MB   count=26395
`ClassLiteral`                                     metadata=0.35MB   fields=0.25MB   count=6286
`TupleType`                                        metadata=0.16MB   fields=0.23MB   count=2919
`place_by_id::interned_arguments`                  metadata=0.75MB   fields=0.22MB   count=13439
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=0.10MB   fields=0.17MB   count=1808
`StringLiteralType`                                metadata=0.59MB   fields=0.17MB   count=10562
`TypeVarInstance`                                  metadata=0.09MB   fields=0.16MB   count=1579
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=0.56MB   fields=0.16MB   count=10054
`Type < 'db >::apply_specialization_::interned_arguments` metadata=0.22MB   fields=0.16MB   count=3970
`Specialization`                                   metadata=0.24MB   fields=0.14MB   count=4347
`GenericAlias`                                     metadata=0.36MB   fields=0.10MB   count=6429
`CallableType`                                     metadata=0.06MB   fields=0.10MB   count=1005
`StarImportPlaceholderPredicate`                   metadata=0.12MB   fields=0.08MB   count=4167
`ModuleNameIngredient`                             metadata=0.15MB   fields=0.07MB   count=2739
`PropertyInstanceType`                             metadata=0.06MB   fields=0.07MB   count=1016
`BoundMethodType`                                  metadata=0.09MB   fields=0.06MB   count=1523
`UnionType`                                        metadata=0.20MB   fields=0.06MB   count=3619
`PatternPredicate`                                 metadata=0.02MB   fields=0.04MB   count=768
`Unpack`                                           metadata=0.03MB   fields=0.04MB   count=637
`ModuleLiteralType`                                metadata=0.11MB   fields=0.03MB   count=1979
`GenericContext`                                   metadata=0.03MB   fields=0.03MB   count=507
`BareTypeAliasType`                                metadata=0.01MB   fields=0.01MB   count=201
`PEP695TypeAliasType`                              metadata=0.02MB   fields=0.01MB   count=272
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.01MB   fields=0.01MB   count=238
`ProtocolInterface`                                metadata=0.01MB   fields=0.00MB   count=143
`BytesLiteralType`                                 metadata=0.01MB   fields=0.00MB   count=137
`TypeIsType`                                       metadata=0.00MB   fields=0.00MB   count=39
`BoundSuperType`                                   metadata=0.00MB   fields=0.00MB   count=26
`Program`                                          metadata=0.00MB   fields=0.00MB   count=1
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths::interned_arguments`     metadata=0.00MB   fields=0.00MB   count=1
```

## Test Plan

cargo build


---

_Label `internal` added by @MichaReiser on 2025-07-10 11:41_

---

_Label `ty` added by @MichaReiser on 2025-07-10 11:41_

---

_Review requested from @carljm by @MichaReiser on 2025-07-10 11:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-10 11:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-10 11:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-10 11:41_

---

_Merged by @MichaReiser on 2025-07-10 11:45_

---

_Closed by @MichaReiser on 2025-07-10 11:45_

---

_Branch deleted on 2025-07-10 11:45_

---

_Comment by @github-actions[bot] on 2025-07-10 11:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-10 11:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

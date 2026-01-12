```yaml
number: 20740
title: "[ty] Make `infer_method_information` less confusing"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/less-confusing-method_information
created_at: 2025-10-07T08:12:16Z
updated_at: 2025-10-07T15:12:09Z
url: https://github.com/astral-sh/ruff/pull/20740
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Make `infer_method_information` less confusing

---

_@sharkdp_

## Summary

`infer_method_information` was previously calling `ClassLiteral::to_class_type`, which uses the default-specialization of a generic class. This specialized `ClassType` was later only used if the class was non-generic, making the specialization irrelevant. The implementation was still a bit confusing, so this PR proposes a way to avoid turning the class literal into a `ClassType`.



---

_Review requested from @carljm by @sharkdp on 2025-10-07 08:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-07 08:12_

---

_Review requested from @dcreager by @sharkdp on 2025-10-07 08:12_

---

_Label `internal` added by @sharkdp on 2025-10-07 08:12_

---

_Label `ty` added by @sharkdp on 2025-10-07 08:12_

---

_Comment by @github-actions[bot] on 2025-10-07 08:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 08:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:70 on 2025-10-07 09:12_

nit

```suggestion
    {
        Type::ClassLiteral(class_literal) => {
            (class_literal, class_literal.generic_context(db).is_some())
        }
        Type::GenericAlias(alias) => (alias.origin(db), true),
        _ => return None,
    };
```

---

_@AlexWaygood approved on 2025-10-07 09:12_

---

_Merged by @sharkdp on 2025-10-07 10:12_

---

_Closed by @sharkdp on 2025-10-07 10:12_

---

_Branch deleted on 2025-10-07 10:12_

---

_Comment by @carljm on 2025-10-07 15:12_

Thank you!

---

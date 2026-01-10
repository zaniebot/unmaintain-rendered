```yaml
number: 2124
title: "re-unify `Type::try_call_constructor` and `Type::bindings`"
type: issue
state: open
author: carljm
labels:
  - calls
assignees: []
created_at: 2025-12-19T23:31:21Z
updated_at: 2025-12-19T23:31:21Z
url: https://github.com/astral-sh/ty/issues/2124
synced_at: 2026-01-10T01:56:41Z
```

# re-unify `Type::try_call_constructor` and `Type::bindings`

---

_Issue opened by @carljm on 2025-12-19 23:31_

We currently do some extensive special-casing in `TypeInferenceBuilder::infer_call_expression_impl`, much of which dates back to working around limitations we no longer have, and can/should now be removed. Part of this special-casing is a dedicated path for constructor calls (`Type::try_call_constructor`), which doesn't use the bindings that would be returned by `Type::bindings` for the same type -- so it's not consistent with what happens if you use `Type::try_call`. This leads to bugs like https://github.com/astral-sh/ty/issues/1446

---

_Added to milestone `Stable` by @carljm on 2025-12-19 23:31_

---

_Label `calls` added by @carljm on 2025-12-19 23:31_

---

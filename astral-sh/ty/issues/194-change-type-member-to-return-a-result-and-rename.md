```yaml
number: 194
title: "Change `Type::member` to return a `Result` and rename it to `try_member`"
type: issue
state: open
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-02-21T09:31:05Z
updated_at: 2025-11-13T06:27:30Z
url: https://github.com/astral-sh/ty/issues/194
synced_at: 2026-01-12T15:54:22Z
```

# Change `Type::member` to return a `Result` and rename it to `try_member`

---

_@MichaReiser_

### Description

Similar to `Type::try_call`, `try_iterate`, `try_bool`, ..., `member` should be renamed to `try_member` and return `Result`. 

* I don't think it should be `LookupResult`: Instead, it should be a result specific to `Type::member` because we may want to preserve additional error information for better diagnostics that isn't relevant when doing other symbol lookups. But I might be wrong here
* I don't think `try_member` should return a `Symbol` because it behaves slightly different from all other type methods in that it doesn't necessarily consider a possibly unbound symbol as an error (but it probably is one for most type checking code). 

We could offer helper methods to easily convert a `Result<Type, MemberError>` to a `Result<Type, LookupError>` or `Symbol` (e.g. implement `From` for `Result<Type, LookupError>` so that the `try` operator "just works").




---

_Renamed from "[red-knot] Change `Type::member` to return a `Result` and rename it to `try_member`" to "Change `Type::member` to return a `Result` and rename it to `try_member`" by @MichaReiser on 2025-05-07 15:26_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:25_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 00:52_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:27_

---

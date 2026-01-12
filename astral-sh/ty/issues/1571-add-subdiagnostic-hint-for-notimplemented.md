```yaml
number: 1571
title: "Add subdiagnostic hint for `NotImplemented()`"
type: issue
state: closed
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-11-15T19:49:36Z
updated_at: 2025-11-19T19:27:14Z
url: https://github.com/astral-sh/ty/issues/1571
synced_at: 2026-01-12T15:54:25Z
```

# Add subdiagnostic hint for `NotImplemented()`

---

_@AlexWaygood_

Callling `NotImplemented()` fails at runtime. If a user tries to call `NotImplemented()` (e.g. in `raise NotImplemented("my message here")`, we currently emit a `call-non-callable` diagnostic:

<img width="1432" height="292" alt="Image" src="https://github.com/user-attachments/assets/dc43d55e-e4a5-4921-863e-c5e0c46fdb28" />


But in this situation, it's almost always the case that the user meant to call `NotImplementedError()` instead (the two are very often confused). What would be great is if we could attach a subdiagnostic saying "Did you mean `NotImplementedError`?".

It's a little bit involved to achieve this, since there are quite a few locations in our codebase where we emit `call-non-callable` diagnostics. It would be nice to refactor that a bit so that all those locations call a single `report_non_callable_called()` function that also attaches the subdiagnostic -- I wouldn't want us to repeat the subdiagnostic logic everywhere we currently emit this diagnostic.

https://github.com/astral-sh/ruff/pull/21475 shows how a similar hint was added for `raise NotImplemented` (which causes us to emit `invalid-raise` rather than `call-non-callable`, so it's a different code path).

---

_Label `help wanted` added by @AlexWaygood on 2025-11-15 19:49_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-15 19:49_

---

_Added to milestone `Stable` by @carljm on 2025-11-15 21:59_

---

_Label `help wanted` removed by @AlexWaygood on 2025-11-18 21:15_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-11-18 21:16_

---

_Closed by @AlexWaygood on 2025-11-19 19:27_

---

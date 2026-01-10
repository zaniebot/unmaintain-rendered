```yaml
number: 21523
title: "[ty] Improve diagnostics when `NotImplemented` is called"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/notimplemented-hint
created_at: 2025-11-19T13:20:47Z
updated_at: 2025-11-19T19:27:14Z
url: https://github.com/astral-sh/ruff/pull/21523
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve diagnostics when `NotImplemented` is called

---

_Pull request opened by @AlexWaygood on 2025-11-19 13:20_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1571.

I realised I was overcomplicating things when I described what we should do in that issue description. The simplest thing to do here is just to special-case call expressions and short-circuit the call-binding machinery entirely if we see it's `NotImplemented` being called. It doesn't really matter if the subdiagnostic doesn't fire when a union is called and one element of the union is `NotImplemented` -- the subdiagnostic doesn't need to be exhaustive; it's just to help people in some common cases.

## Test Plan

Added snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-11-19 13:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-19 13:20_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-19 13:20_

---

_Label `ty` added by @AlexWaygood on 2025-11-19 13:20_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-19 13:20_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 13:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-19 13:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/demos/excelRTDServer.py:239:15: error[call-non-callable] Object of type `NotImplementedType` is not callable
+ com/win32com/demos/excelRTDServer.py:239:15: error[call-non-callable] `NotImplemented` is not callable - did you mean `NotImplementedError`?
- com/win32com/demos/excelRTDServer.py:278:15: error[call-non-callable] Object of type `NotImplementedType` is not callable
+ com/win32com/demos/excelRTDServer.py:278:15: error[call-non-callable] `NotImplemented` is not callable - did you mean `NotImplementedError`?


```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-11-19 19:25_

---

_Merged by @AlexWaygood on 2025-11-19 19:27_

---

_Closed by @AlexWaygood on 2025-11-19 19:27_

---

_Branch deleted on 2025-11-19 19:27_

---

```yaml
number: 21935
title: "[ty] Remove now-unnecessary Divergent check"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/remove-divergent-check
created_at: 2025-12-12T08:06:11Z
updated_at: 2025-12-13T15:32:11Z
url: https://github.com/astral-sh/ruff/pull/21935
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Remove now-unnecessary Divergent check

---

_@sharkdp_

## Summary

This check is not necessary thanks to https://github.com/astral-sh/ruff/pull/21906.


---

_Review requested from @carljm by @sharkdp on 2025-12-12 08:06_

---

_Label `internal` added by @sharkdp on 2025-12-12 08:06_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-12 08:06_

---

_Review requested from @dcreager by @sharkdp on 2025-12-12 08:06_

---

_Label `ty` added by @sharkdp on 2025-12-12 08:06_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @sharkdp on 2025-12-12 08:12_

This is not necessary for our own tests anymore, but apparently still necessary on the pydantic examples.

---

_Closed by @sharkdp on 2025-12-12 08:12_

---

_Reopened by @sharkdp on 2025-12-13 09:11_

---

_@AlexWaygood approved on 2025-12-13 12:13_

---

_Merged by @sharkdp on 2025-12-13 15:32_

---

_Closed by @sharkdp on 2025-12-13 15:32_

---

_Branch deleted on 2025-12-13 15:32_

---

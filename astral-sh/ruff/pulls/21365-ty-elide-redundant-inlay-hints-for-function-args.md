```yaml
number: 21365
title: "[ty] elide redundant inlay hints for function args"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/inlay-less
created_at: 2025-11-10T15:51:33Z
updated_at: 2025-11-10T16:42:13Z
url: https://github.com/astral-sh/ruff/pull/21365
synced_at: 2026-01-10T16:53:55Z
```

# [ty] elide redundant inlay hints for function args

---

_Pull request opened by @Gankra on 2025-11-10 15:51_

This elides the following inlay hints:

```py
foo([x=]x)
foo([x=]y.x)
foo([x=]x[0])
foo([x=]x(...))

# composes to complex situations
foo([x=]y.x(..)[0])
```

Fixes https://github.com/astral-sh/ty/issues/1514



---

_Review requested from @carljm by @Gankra on 2025-11-10 15:51_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-10 15:51_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-10 15:51_

---

_Review requested from @sharkdp by @Gankra on 2025-11-10 15:51_

---

_Review requested from @dcreager by @Gankra on 2025-11-10 15:51_

---

_Label `server` added by @Gankra on 2025-11-10 15:51_

---

_Label `ty` added by @Gankra on 2025-11-10 15:51_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 15:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-10 15:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-11-10 16:05_

pygame world peace achieved 😌 
<img width="425" height="53" alt="Screenshot 2025-11-10 at 10 59 34 AM" src="https://github.com/user-attachments/assets/01f8192b-4efe-4ee6-bed5-27b0a7b0ba13" />

(yes there's still a complain to be had in this screenshot: https://github.com/astral-sh/ty/issues/1516)

---

_@MichaReiser approved on 2025-11-10 16:41_

Nice!

---

_Merged by @Gankra on 2025-11-10 16:42_

---

_Closed by @Gankra on 2025-11-10 16:42_

---

_Branch deleted on 2025-11-10 16:42_

---

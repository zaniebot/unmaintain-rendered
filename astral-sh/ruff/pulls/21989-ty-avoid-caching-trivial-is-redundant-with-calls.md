```yaml
number: 21989
title: "[ty] Avoid caching trivial is-redundant-with calls"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/avoid-caching-trivial-is-redundant-with
created_at: 2025-12-15T17:24:49Z
updated_at: 2025-12-15T17:45:05Z
url: https://github.com/astral-sh/ruff/pull/21989
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Avoid caching trivial is-redundant-with calls

---

_@MichaReiser_

_No description provided._

---

_Label `ty` added by @MichaReiser on 2025-12-15 17:24_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 17:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-15 17:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅


<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~80MB
+     memo metadata = ~76MB


```

</details>




---

_Comment by @MichaReiser on 2025-12-15 17:33_

This seems worth it even just from a memory standpoint

---

_@Gankra approved on 2025-12-15 17:34_

Wow yeah great win

---

_Marked ready for review by @MichaReiser on 2025-12-15 17:44_

---

_Review requested from @carljm by @MichaReiser on 2025-12-15 17:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-15 17:44_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-15 17:44_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-15 17:44_

---

_Comment by @MichaReiser on 2025-12-15 17:44_

Looks like a 1-2% performance improvement. I take it

---

_Label `internal` added by @MichaReiser on 2025-12-15 17:45_

---

_Merged by @MichaReiser on 2025-12-15 17:45_

---

_Closed by @MichaReiser on 2025-12-15 17:45_

---

_Branch deleted on 2025-12-15 17:45_

---

```yaml
number: 19214
title: "[ty] Improved diagnostic for reassignments of `Final` symbols"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/touchup-final-diagnostic
created_at: 2025-07-08T18:14:40Z
updated_at: 2025-07-08T18:29:09Z
url: https://github.com/astral-sh/ruff/pull/19214
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Improved diagnostic for reassignments of `Final` symbols

---

_@sharkdp_

## Summary

Implement [this suggestion](https://github.com/astral-sh/ruff/pull/19178#discussion_r2192658146) by @AlexWaygood.

![image](https://github.com/user-attachments/assets/f183d691-ef6e-43a2-b005-3a32205bc408)


---

_Review requested from @carljm by @sharkdp on 2025-07-08 18:14_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-08 18:14_

---

_Review requested from @dcreager by @sharkdp on 2025-07-08 18:14_

---

_Label `internal` added by @sharkdp on 2025-07-08 18:14_

---

_Label `ty` added by @sharkdp on 2025-07-08 18:14_

---

_Label `diagnostics` added by @sharkdp on 2025-07-08 18:14_

---

_@sharkdp reviewed on 2025-07-08 18:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1698 on 2025-07-08 18:17_

This is a fallback that currently can't be triggered, I believe, because there are no other annotated definitions that would allow a `Final` annotation. I guess it doesn't hurt to leave this in here, but I can also remove it.

In particular, function parameters are not allowed to be annotated with `Final`:

> Final may only be used in assignments or variable annotations. Using it in any other position is an error. In particular, Final can’t be used in annotations for function arguments

https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final

This is something that we still have to implement (added a note to the ticket for `typing.Final`).

---

_@AlexWaygood approved on 2025-07-08 18:17_

😍

---

_Comment by @github-actions[bot] on 2025-07-08 18:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @sharkdp on 2025-07-08 18:29_

---

_Closed by @sharkdp on 2025-07-08 18:29_

---

_Branch deleted on 2025-07-08 18:29_

---

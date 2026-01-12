```yaml
number: 20302
title: "[ty] Fix signature of `NamedTupleLike._make`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/namedtuplelike
created_at: 2025-09-08T10:59:25Z
updated_at: 2025-09-08T12:53:19Z
url: https://github.com/astral-sh/ruff/pull/20302
synced_at: 2026-01-12T15:56:58Z
```

# [ty] Fix signature of `NamedTupleLike._make`

---

_@sharkdp_

_No description provided._

---

_Review requested from @carljm by @sharkdp on 2025-09-08 10:59_

---

_Review requested from @MichaReiser by @sharkdp on 2025-09-08 10:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-08 10:59_

---

_Review requested from @dcreager by @sharkdp on 2025-09-08 10:59_

---

_Label `ty` added by @sharkdp on 2025-09-08 10:59_

---

_Review request for @AlexWaygood removed by @sharkdp on 2025-09-08 10:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-08 10:59_

---

_Comment by @github-actions[bot] on 2025-09-08 11:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @AlexWaygood on 2025-09-08 11:02_

oops, yes, it was meant to be `cls: type[Self]`. Another data point for the argument that we should try to do as little special-casing as possible, and rely on typeshed as much as we can...

---

_@AlexWaygood reviewed on 2025-09-08 11:03_

---

_Review comment by @AlexWaygood on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:96 on 2025-09-08 11:03_

this also would make sense for now, I think? But I'm also fine with what you have if #18007 is _just_ about to be merged ;)

```suggestion
    def _make(cls: type[Self], iterable: Iterable[Any]) -> Self: ...
```

---

_@AlexWaygood approved on 2025-09-08 11:03_

---

_Comment by @github-actions[bot] on 2025-09-08 11:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:96 on 2025-09-08 11:06_

I guess it's maybe a bit of a moot point while https://github.com/astral-sh/ty/issues/501 is still unimplemented, though

---

_@AlexWaygood reviewed on 2025-09-08 11:06_

---

_@sharkdp reviewed on 2025-09-08 11:07_

---

_Review comment by @sharkdp on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:96 on 2025-09-08 11:07_

Changed to an explicit `type[Self]` for now.

---

_Renamed from "[ty] Fix signature of NamedTupleLike._make" to "[ty] Fix signature of `NamedTupleLike._make`" by @sharkdp on 2025-09-08 11:09_

---

_Merged by @sharkdp on 2025-09-08 12:53_

---

_Closed by @sharkdp on 2025-09-08 12:53_

---

_Branch deleted on 2025-09-08 12:53_

---

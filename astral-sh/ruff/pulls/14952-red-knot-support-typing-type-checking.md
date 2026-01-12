```yaml
number: 14952
title: "[red-knot] Support `typing.TYPE_CHECKING`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typing-type_checking
created_at: 2024-12-13T08:52:17Z
updated_at: 2024-12-13T09:26:19Z
url: https://github.com/astral-sh/ruff/pull/14952
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Support `typing.TYPE_CHECKING`

---

_@sharkdp_

## Summary

Add support for `typing.TYPE_CHECKING` and `typing_extensions.TYPE_CHECKING`.

relates to: https://github.com/astral-sh/ruff/issues/14170

## Test Plan

New Markdown-based tests

---

_Label `red-knot` added by @sharkdp on 2024-12-13 08:52_

---

_Review requested from @carljm by @sharkdp on 2024-12-13 08:52_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-13 08:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-13 08:52_

---

_@AlexWaygood reviewed on 2024-12-13 08:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:123 on 2024-12-13 08:56_

Could we check that `name` is equal to `"TYPE_CHECKING"` _before_ retrieving the module using `file_to_module()`? A simple string comparison should be much cheaper than the lookup in the salsa db, I think

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:124 on 2024-12-13 08:58_

Worth adding a comment that `typing_extensions` just re-exports the symbol from `typing`?

---

_@AlexWaygood reviewed on 2024-12-13 08:58_

---

_Comment by @github-actions[bot] on 2024-12-13 08:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-12-13 09:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:123 on 2024-12-13 09:08_

Yes — that also looks a bit cleaner now. Thanks.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:127 on 2024-12-13 09:11_

I think it should be possible to compare ModuleNames with strings directly, without the `as_str()` call. You might need to borrow the l.h.s.?

https://github.com/astral-sh/ruff/blob/f52b1f4a4d076fc9e17d8341f2358a1522bd2f9e/crates/red_knot_python_semantic/src/module_name.rs#L200

---

_@AlexWaygood approved on 2024-12-13 09:11_

Nice 

---

_Merged by @sharkdp on 2024-12-13 09:24_

---

_Closed by @sharkdp on 2024-12-13 09:24_

---

_Branch deleted on 2024-12-13 09:24_

---

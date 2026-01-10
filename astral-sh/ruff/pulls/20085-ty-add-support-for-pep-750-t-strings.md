```yaml
number: 20085
title: "[ty] Add support for PEP 750 t-strings "
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-t-strings
created_at: 2025-08-25T17:44:47Z
updated_at: 2025-08-25T18:51:48Z
url: https://github.com/astral-sh/ruff/pull/20085
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add support for PEP 750 t-strings 

---

_Pull request opened by @dylwil3 on 2025-08-25 17:44_

This PR attempts to add support for inferring `string.templatelib.Template` for t-string literals.

First PR of this kind so apologies in advance if I've made a mess of things! Also this is low priority so feel free to ignore. 

Diff is relatively small but feel free to review commit-by-commit if it's more convenient.

Also tested LSP:
![demo](https://github.com/user-attachments/assets/25223d08-cd63-4dee-8d17-2c8f1e817e12)




---

_Review requested from @carljm by @dylwil3 on 2025-08-25 17:44_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-08-25 17:44_

---

_Review requested from @sharkdp by @dylwil3 on 2025-08-25 17:44_

---

_Review requested from @dcreager by @dylwil3 on 2025-08-25 17:44_

---

_Label `ty` added by @dylwil3 on 2025-08-25 17:44_

---

_@dylwil3 reviewed on 2025-08-25 17:46_

---

_Review comment by @dylwil3 on `crates/ty_python_semantic/src/types/class.rs`:4061 on 2025-08-25 17:46_

Not sure if you'd like this to be the name or if I should use the full `string.templatelib.Template` here. A point of confusion is that (very unfortunately) `string.Template` exists.

---

_Comment by @github-actions[bot] on 2025-08-25 17:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@dylwil3 reviewed on 2025-08-25 17:46_

---

_Review comment by @dylwil3 on `crates/ty_python_semantic/src/types/class.rs`:5117 on 2025-08-25 17:46_

Let me know if you'd like me to handle this differently, and/or add an assertion to remind us to modify this once the latest supported version is Python 3.14

---

_Comment by @github-actions[bot] on 2025-08-25 17:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/t_strings.md`:5 on 2025-08-25 17:53_

what's the reason for switching off the formatter here?

---

_Renamed from "[`ty`] Add support for PEP 750 t-strings " to "[ty] Add support for PEP 750 t-strings " by @dylwil3 on 2025-08-25 17:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:02_

`Template` is [marked as `@final` in typeshed](https://github.com/python/typeshed/blob/91e2ed0953592795fd8c29e3005a1315bf652ffc/stdlib/string/templatelib.pyi#L7-L8), so I don't think we also need to consider it a disjoint base (see the comment a couple of lines below this one) -- it'll just result in us emitting two diagnostics if you tried to inherit from it, rather than one diagnostic. One seems enough :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4061 on 2025-08-25 18:03_

this is correct -- it's just meant to be the `__name__` here, not the fullname!

---

_@AlexWaygood approved on 2025-08-25 18:06_

Looks great!

---

_@dylwil3 reviewed on 2025-08-25 18:11_

---

_Review comment by @dylwil3 on `crates/ty_python_semantic/resources/mdtest/t_strings.md`:5 on 2025-08-25 18:11_

`black` does not yet support Python 3.14 so it errors here

---

_@AlexWaygood reviewed on 2025-08-25 18:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/t_strings.md`:5 on 2025-08-25 18:16_

ahhh, makes sense. Can you add a comment?

---

_@AlexWaygood reviewed on 2025-08-25 18:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:40_

...and I just deleted this method entirely in https://github.com/astral-sh/ruff/pull/20084, so the question is now moot 😆 sorry for giving you merge conflicts!

---

_@dylwil3 reviewed on 2025-08-25 18:41_

---

_Review comment by @dylwil3 on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:41_

no worries!

---

_Merged by @dylwil3 on 2025-08-25 18:49_

---

_Closed by @dylwil3 on 2025-08-25 18:49_

---

_Branch deleted on 2025-08-25 18:51_

---

```yaml
number: 22006
title: "[ty] Improve syntax-highlighting of constants"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/constant_highlight
created_at: 2025-12-16T14:13:57Z
updated_at: 2025-12-16T14:23:42Z
url: https://github.com/astral-sh/ruff/pull/22006
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve syntax-highlighting of constants

---

_Pull request opened by @Gankra on 2025-12-16 14:13_

## Summary

BLAH1 getting different highlighting/classification from BLAH is very distracting and undesirable.

---

_Review requested from @carljm by @Gankra on 2025-12-16 14:13_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-16 14:13_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-16 14:13_

---

_Review requested from @sharkdp by @Gankra on 2025-12-16 14:13_

---

_Review requested from @dcreager by @Gankra on 2025-12-16 14:13_

---

_Label `server` added by @Gankra on 2025-12-16 14:13_

---

_Label `ty` added by @Gankra on 2025-12-16 14:13_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 14:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 14:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5124 diagnostics
+ Found 5123 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] improve syntax-highlighting of constants" to "[ty] Improve syntax-highlighting of constants" by @Gankra on 2025-12-16 14:18_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/semantic_tokens.rs`:2257 on 2025-12-16 14:20_

...we classify these as readonly? That feels like a pyrightism that crept into the code somewhere. We might want to reconsider that 🤔

Pyright implements a heuristic where SCREAMING_SNAKE_CASE constants are always treated as implicitly read-only. But in terms of the semantics _we_ give variables, we currently only treat variables as read-only if they are explicitly annotated as `Final` (as per the spec).

If this classification only affects the way these variables are syntax-highlighted by the editor, though, then maybe it's fine

---

_@AlexWaygood approved on 2025-12-16 14:21_

---

_@Gankra reviewed on 2025-12-16 14:23_

---

_Review comment by @Gankra on `crates/ty_ide/src/semantic_tokens.rs`:2257 on 2025-12-16 14:23_

Yeah I believe it's purely syntax highlighting.

---

_Merged by @Gankra on 2025-12-16 14:23_

---

_Closed by @Gankra on 2025-12-16 14:23_

---

_Branch deleted on 2025-12-16 14:23_

---

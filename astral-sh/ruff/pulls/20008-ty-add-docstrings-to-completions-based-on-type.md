```yaml
number: 20008
title: "[ty] add docstrings to completions based on type"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/doc-comp
created_at: 2025-08-20T17:49:00Z
updated_at: 2025-08-20T21:00:11Z
url: https://github.com/astral-sh/ruff/pull/20008
synced_at: 2026-01-12T15:56:52Z
```

# [ty] add docstrings to completions based on type

---

_@Gankra_

This is a fairly simple but effective way to add docstrings to like 95% of completions from initial experimentation.

Fixes https://github.com/astral-sh/ty/issues/1036

Although ironically this approach *does not* work specifically for `print` and I haven't looked into why.

---

_Review requested from @carljm by @Gankra on 2025-08-20 17:49_

---

_Label `server` added by @Gankra on 2025-08-20 17:49_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-20 17:49_

---

_Review requested from @sharkdp by @Gankra on 2025-08-20 17:49_

---

_Label `ty` added by @Gankra on 2025-08-20 17:49_

---

_Review requested from @dcreager by @Gankra on 2025-08-20 17:49_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-20 17:49_

---

_Comment by @github-actions[bot] on 2025-08-20 17:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 17:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @Gankra on 2025-08-20 17:53_


https://github.com/user-attachments/assets/d335e367-5114-45cf-8732-627f7d783934



---

_Review comment by @MichaReiser on `crates/ty_wasm/src/lib.rs`:424 on 2025-08-20 17:57_

It would be nice to also hook up documentation in the playground. You have to add a new `documentation` field to `Completion`, then propagate the field here

https://github.com/astral-sh/ruff/blob/8c0743df97dc7afa1deab7506b322551f2e392bb/playground/ty/src/Editor/Editor.tsx#L321

---

_@MichaReiser reviewed on 2025-08-20 17:57_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:59 on 2025-08-20 17:58_

Should we make this a field on `Completion` instead that defaults to `None`: Given that `completion` is the only method constructing the completions

---

_@MichaReiser approved on 2025-08-20 17:58_

Nice

---

_@Gankra reviewed on 2025-08-20 18:09_

---

_Review comment by @Gankra on `crates/ty_ide/src/completion.rs`:59 on 2025-08-20 18:09_

Currently that doesn't work out because Docstring is defined in ty_ide and Completion is defined in ty_python_semantic. The crate boundaries have gotten quite wobbly 😅 

---

_@Gankra reviewed on 2025-08-20 18:20_

---

_Review comment by @Gankra on `crates/ty_wasm/src/lib.rs`:424 on 2025-08-20 18:20_

I think done? Never tested the playground.

---

_Merged by @Gankra on 2025-08-20 21:00_

---

_Closed by @Gankra on 2025-08-20 21:00_

---

_Branch deleted on 2025-08-20 21:00_

---

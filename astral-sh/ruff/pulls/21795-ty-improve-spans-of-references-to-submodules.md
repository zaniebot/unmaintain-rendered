```yaml
number: 21795
title: "[ty] Improve spans of references to submodules imported in an `__init__.py`"
type: pull_request
state: open
author: Gankra
labels:
  - server
  - ty
assignees: []
base: main
head: gankra/fix-span
created_at: 2025-12-04T17:05:34Z
updated_at: 2025-12-05T18:03:29Z
url: https://github.com/astral-sh/ruff/pull/21795
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Improve spans of references to submodules imported in an `__init__.py`

---

_@Gankra_

Good ol' `DefinitionKind::ImportFromSubmodule`

* Improves https://github.com/astral-sh/ty/issues/1759

---

_Review requested from @carljm by @Gankra on 2025-12-04 17:05_

---

_Label `server` added by @Gankra on 2025-12-04 17:05_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-04 17:05_

---

_Review requested from @sharkdp by @Gankra on 2025-12-04 17:05_

---

_Review requested from @dcreager by @Gankra on 2025-12-04 17:05_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-04 17:05_

---

_Label `ty` added by @Gankra on 2025-12-04 17:05_

---

_Renamed from "[ty] Fix spans of references to submodules imported in an `__init__.py`" to "[ty] Improve spans of references to submodules imported in an `__init__.py`" by @Gankra on 2025-12-04 17:05_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 17:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 17:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index/definition.rs`:1071 on 2025-12-04 17:15_

This is probably Parser Crimes and there's probably a better API for doing this but this is my absolute favourite trick and I'll use it any time I can

---

_@Gankra reviewed on 2025-12-04 17:15_

---

_@MichaReiser reviewed on 2025-12-04 18:27_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/definition.rs`:1071 on 2025-12-04 18:27_

Took me a while to understand what's happening here, especially the pointer stuff. This could be rewritten to:

```
        let module_ident = self.module(module);
        let module_str = module_ident.as_str();

        let Some((end_offset, _)) = module_str.match_indices('.').nth(self.module_index) else {
            // This shouldn't happen but just in case, provide a safe default
            return module_ident.range();
        };

        let (component_str, _) = module_str.split_at(end_offset);

        let Ok(end_offset) = TextSize::try_from(end_offset) else {
            // This shouldn't happen but just in case, provide a safe default
            return module_ident.range();
        };
        let start_offset = end_offset - component_str.text_len();
        TextRange::new(start_offset, end_offset) + module_ident.start()
```

Or we should add a short comment explaining what we're doing here.

---

_@MichaReiser reviewed on 2025-12-04 18:28_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/definition.rs`:1071 on 2025-12-04 18:28_

I don't think the `saturating_add` gives you much here, as we are as likely to panic somewhere in the text range conversion if the new range ends up being between two char boundaries. In which case I'd prefer if it panicked in the add

---

_@MichaReiser approved on 2025-12-04 18:28_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-05 18:03_

---

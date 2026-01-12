```yaml
number: 21155
title: "[ty] Remove mentions of VS Code from server logs"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: ty-ide-logs
created_at: 2025-10-31T00:12:39Z
updated_at: 2025-11-06T11:48:49Z
url: https://github.com/astral-sh/ruff/pull/21155
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Remove mentions of VS Code from server logs

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Some of the logs i received in zed mentioned "VS Code", and one log displayed site packages paths like: `SitePackagesPaths({"..", ".."})`, which doesn't look great.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-10-31 00:12_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-10-31 00:12_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-10-31 00:12_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-10-31 00:12_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-10-31 00:12_

---

_Comment by @github-actions[bot] on 2025-10-31 00:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 00:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @AlexWaygood on 2025-10-31 00:19_

---

_Label `server` added by @AlexWaygood on 2025-10-31 00:19_

---

_Label `ty` added by @AlexWaygood on 2025-10-31 00:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:114 on 2025-10-31 12:43_

I think we should implement `Display` instead of `Debug` if we care about how this is formatted. It more clearly communicates that the formatting is user-facing.

---

_@MichaReiser reviewed on 2025-10-31 12:52_

Thank you. Yeah, Zed started using my "VS code only" feature 😆 

I think we should also change `ValueSource::VSCodeExtension` to `ValueSource::Editor` or similar.

---

_Comment by @MatthewMckee4 on 2025-10-31 13:08_

We also have `PythonVersionSource::PythonVSCodeExtension` and `SysPrefixPathOrigin::PythonVSCodeExtension`. Should these be updated too?

---

_Comment by @MichaReiser on 2025-10-31 13:47_

Oh yes, those should be updated too

---

_Comment by @MatthewMckee4 on 2025-11-01 15:13_

does `ValueSource::LSPConfiguration` sound better?

To me `LSPConfiguration` seems like a generalization of `Editor`, but I'm not sure what is better, its not like anyone is using the lsp not through an editor, so perhaps editor is better. I think all 3 of these should match too.

---

_Renamed from "Update ty server logs" to "[ty] Update ty server logs" by @MatthewMckee4 on 2025-11-02 12:07_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-02 23:28_

---

_@MichaReiser reviewed on 2025-11-03 14:33_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/value.rs`:32 on 2025-11-03 14:33_

```suggestion
    /// The value comes from the user's editor,
    /// while it's left open if specified as a setting
    /// or if the value was auto-discovered by the editor
    /// (e.g., the Python environment)
    Editor,
```

---

_@MichaReiser reviewed on 2025-11-03 14:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/diagnostic.rs`:94 on 2025-11-03 14:34_

```suggestion
                because it's the version of the selected Python interpreter in your editor",
```

---

_@MichaReiser reviewed on 2025-11-03 14:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:116 on 2025-11-03 14:38_

```suggestion
    /// The value comes from the user's editor,
    /// while it's left open if specified as a setting
    /// or if the value was auto-discovered by the editor
    /// (e.g., the Python environment)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:1629 on 2025-11-03 14:39_

```suggestion
            Self::Editor => f.write_str("selected interpreter in your editor"),
```

---

_@MichaReiser reviewed on 2025-11-03 14:39_

---

_Renamed from "[ty] Update ty server logs" to "[ty] Remove mentions of VS Code from server logs" by @MichaReiser on 2025-11-03 14:40_

---

_Comment by @MichaReiser on 2025-11-03 14:40_

Thank you

---

_Merged by @MichaReiser on 2025-11-03 14:49_

---

_Closed by @MichaReiser on 2025-11-03 14:49_

---

_Branch deleted on 2025-11-06 11:48_

---

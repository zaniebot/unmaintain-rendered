```yaml
number: 533
title: "Add support for `nonlocal` and `global` bindings to find-references"
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-05-28T11:31:31Z
updated_at: 2025-11-25T18:50:48Z
url: https://github.com/astral-sh/ty/issues/533
synced_at: 2026-01-10T01:58:59Z
```

# Add support for `nonlocal` and `global` bindings to find-references

---

_Issue opened by @MichaReiser on 2025-05-28 11:31_

Add support for finding all references to a symbol in the project. See [find reference](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_references) request

---

_Added to milestone `GA` by @MichaReiser on 2025-05-28 11:31_

---

_Label `server` added by @MichaReiser on 2025-05-28 11:31_

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:48_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:48_

---

_Assigned to @UnboundVariable by @MichaReiser on 2025-07-23 09:17_

---

_Comment by @UnboundVariable on 2025-07-24 20:14_

I think this feature is now complete except for symbols that are bound using a `nonlocal` or `global` statement. Tests are in place for these cases, but they're currently disabled (with TODO comments [here](https://github.com/astral-sh/ruff/blob/4bc34b82efacb3bd62f16940ab2517987edbc61c/crates/ty_ide/src/goto_references.rs#L148) and [here](https://github.com/astral-sh/ruff/blob/4bc34b82efacb3bd62f16940ab2517987edbc61c/crates/ty_ide/src/goto_references.rs#L272)). We can fix this once the semantic index collects all definitions for these symbols in their "owning scope".

---

_Comment by @carljm on 2025-08-15 01:21_

Retitling the issue to capture what is left to do here.

---

_Renamed from "Find references" to "Add support for `nonlocal` and `global` bindings to find-references" by @carljm on 2025-08-15 01:22_

---

_Unassigned @UnboundVariable by @carljm on 2025-08-15 01:22_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:03_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:03_

---

_Comment by @Gankra on 2025-11-25 18:50_

Done in https://github.com/astral-sh/ruff/pull/21616

---

_Closed by @Gankra on 2025-11-25 18:50_

---

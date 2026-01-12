```yaml
number: 1759
title: "LSP doesn't really understand `DefinitionKind::ImportFromSubmodule`"
type: issue
state: open
author: Gankra
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-04T16:02:54Z
updated_at: 2026-01-09T04:11:25Z
url: https://github.com/astral-sh/ty/issues/1759
synced_at: 2026-01-12T15:54:25Z
```

# LSP doesn't really understand `DefinitionKind::ImportFromSubmodule`

---

_@Gankra_

### Summary

This covers the implicit `subpkg = mypackage.subpkg` local assignment that `from .subpkg.whatever import blah` creates in an `__init__.py`.

In https://github.com/astral-sh/ruff/pull/21793 I added several tests for this and marked the bad results as `TODO(submodule-imports)`

✅ goto-type and hover work great, because they're just asking the inference engine or correctly using the fact that hovering the LHS of import-from is always modules.

🆗  goto-declaration mostly works, there's one "hmm what SHOULD this do" case and one "oh I think we're just seeing broken handling of imports writ-large" 

~~The span is overly broad. This is because the `Definition` actually claims the entire `from..import..` AST node. Unfortunately even if we made it only claim the LHS Identifier it would still be overly broad (`from .x.y` would highlight `x.y` instead of just `x`). So either way the LSP probably wants to special-case the span here.~~ (fixed in https://github.com/astral-sh/ruff/pull/21795)

❌ find-references/rename doesn't really understand it at all.

### Version

_No response_

---

_Label `bug` added by @Gankra on 2025-12-04 16:03_

---

_Label `server` added by @Gankra on 2025-12-04 16:03_

---

_Added to milestone `Stable` by @Gankra on 2025-12-04 16:05_

---

_Comment by @Gankra on 2025-12-04 16:06_

I'm gonna call this P1 because any time renames go awry it's infinite sadness.

---

_Assigned to @Gankra by @Gankra on 2026-01-09 04:11_

---

_Unassigned @Gankra by @Gankra on 2026-01-09 04:11_

---

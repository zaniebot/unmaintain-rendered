```yaml
number: 22512
title: "[ty] Improve rendering of ReST directives in docstrings"
type: pull_request
state: open
author: AryanBagade
labels:
  - server
  - ty
assignees: []
base: main
head: feat/improve-rest-directive-rendering
created_at: 2026-01-12T00:10:54Z
updated_at: 2026-01-12T08:24:52Z
url: https://github.com/astral-sh/ruff/pull/22512
synced_at: 2026-01-12T15:57:51Z
```

# [ty] Improve rendering of ReST directives in docstrings

---

_@AryanBagade_

## Summary
Improves the rendering of ReST version directives in docstrings to use human-readable phrases matching Sphinx/Pylance output.
**Before:**
  - `**version-added:** *3.0*`
  - `**version-changed:** *4.0*`
  - `**deprecated:** *1.2.3*`
  
**After:**
  - `**Added in version 3.0:**`
  - `**Changed in version 4.0:**`
  - `**Deprecated since version 1.2.3:**`

Mapping follows Sphinx's documentation:
  - `versionadded` / `version-added` → "Added in version"
  - `versionchanged` / `version-changed` → "Changed in version"
  - `deprecated` / `version-deprecated` → "Deprecated since version"
  - `versionremoved` / `version-removed` → "Removed in version"

  Fixes https://github.com/astral-sh/ty/issues/2147

  ## Test Plan

  - Updated existing snapshot tests (`version_blocks`, `deprecated_prefix_gunk`)
  - All 1078 ty_ide tests pass
  - `cargo clippy` passes with no warnings


---

_Review requested from @carljm by @AryanBagade on 2026-01-12 00:10_

---

_Review requested from @MichaReiser by @AryanBagade on 2026-01-12 00:10_

---

_Review requested from @AlexWaygood by @AryanBagade on 2026-01-12 00:10_

---

_Review requested from @sharkdp by @AryanBagade on 2026-01-12 00:10_

---

_Review requested from @dcreager by @AryanBagade on 2026-01-12 00:10_

---

_Review requested from @Gankra by @AryanBagade on 2026-01-12 00:10_

---

_Label `server` added by @AlexWaygood on 2026-01-12 00:12_

---

_Label `ty` added by @AlexWaygood on 2026-01-12 00:12_

---

_Assigned to @Gankra by @MichaReiser on 2026-01-12 08:16_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-12 08:24_

---

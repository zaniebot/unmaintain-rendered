```yaml
number: 1305
title: "Bad autocomplete suggestions given for `from .<CURSOR>` (relative imports)"
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-10-04T16:40:41Z
updated_at: 2025-11-21T15:52:06Z
url: https://github.com/astral-sh/ty/issues/1305
synced_at: 2026-01-10T01:58:59Z
```

# Bad autocomplete suggestions given for `from .<CURSOR>` (relative imports)

---

_Issue opened by @AlexWaygood on 2025-10-04 16:40_

### Summary

For `from .<CURSOR>` in the global scope, ty does not appear to give any autocomplete suggestions currently. I would expect it to list submodules of the current package -- in this context, the only available submodule is `.foo`:

<img width="1244" height="370" alt="Image" src="https://github.com/user-attachments/assets/61424b41-c96f-42a3-8bd3-5d1a6a093b9a" />

Inside function scopes, ty does give autocomplete suggestions after `from .`, but they refer to symbols in scope rather than submodules:

<img width="1444" height="940" alt="Image" src="https://github.com/user-attachments/assets/3f01d3aa-9ba1-4ea7-9820-852afc0a83c9" />

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-10-04 16:40_

---

_Label `completions` added by @AlexWaygood on 2025-10-04 16:40_

---

_Comment by @MichaReiser on 2025-11-14 08:40_

CC: @BurntSushi 

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:40_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-11-14 13:30_

---

_Comment by @BurntSushi on 2025-11-21 15:52_

Closed by https://github.com/astral-sh/ruff/pull/21547

---

_Closed by @BurntSushi on 2025-11-21 15:52_

---

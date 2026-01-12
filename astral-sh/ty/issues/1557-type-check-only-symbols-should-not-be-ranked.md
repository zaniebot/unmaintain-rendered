```yaml
number: 1557
title: "`@type_check_only` symbols should not be ranked below other symbols in completion suggestions if the cursor is in an `if TYPE_CHECKING` block or stub file"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - typing semantics
  - completions
assignees: []
created_at: 2025-11-14T16:28:33Z
updated_at: 2025-11-14T17:03:51Z
url: https://github.com/astral-sh/ty/issues/1557
synced_at: 2026-01-12T15:54:25Z
```

# `@type_check_only` symbols should not be ranked below other symbols in completion suggestions if the cursor is in an `if TYPE_CHECKING` block or stub file

---

_@AlexWaygood_

If the user's cursor is in either of these contexts, it's okay to use or import a symbol that doesn't exist at runtime. The stub-file part of this is easy: we just need to check whether the file extension is `.pyi`. The `TYPE_CHECKING` part of this can't be done well without implementing https://github.com/astral-sh/ty/issues/1553 first, however.

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-14 16:28_

---

_Label `completions` added by @AlexWaygood on 2025-11-14 16:28_

---

_Label `server` added by @AlexWaygood on 2025-11-14 17:03_

---

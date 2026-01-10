```yaml
number: 757
title: do not provide underscore-prefixed names from typeshed builtins as autocomplete suggestions
type: issue
state: closed
author: carljm
labels:
  - server
  - completions
assignees: []
created_at: 2025-07-02T19:50:22Z
updated_at: 2025-10-03T12:19:33Z
url: https://github.com/astral-sh/ty/issues/757
synced_at: 2026-01-10T02:06:24Z
```

# do not provide underscore-prefixed names from typeshed builtins as autocomplete suggestions

---

_Issue opened by @carljm on 2025-07-02 19:50_

### Summary

Currently if I type `_T` in a new empty file and request completions, ty will suggest various options (`_T`, `_T1`, `_T2`, ..., `_T_contra`, ...) all of which appear to come from typeshed's `builtins.pyi`.

None of these names are real runtime builtins; they should not be considered exported from `builtins.pyi`, and we should not provide them as autocomplete suggestions.

I think the rule here should be that we ignore anything with an underscore prefix from `builtins.pyi`.

### Version

_No response_

---

_Label `server` added by @carljm on 2025-07-02 19:50_

---

_Added to milestone `Beta` by @carljm on 2025-07-02 19:50_

---

_Comment by @T-256 on 2025-07-04 21:50_

fixed by https://github.com/astral-sh/ruff/pull/19121

---

_Closed by @carljm on 2025-07-04 22:02_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---

```yaml
number: 17876
title: "[ty] Duplicate error from `from X import A, B` where X is unresolved"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - ty
assignees: []
created_at: 2025-05-06T01:46:09Z
updated_at: 2025-05-06T11:37:11Z
url: https://github.com/astral-sh/ruff/issues/17876
synced_at: 2026-01-12T15:54:56Z
```

# [ty] Duplicate error from `from X import A, B` where X is unresolved

---

_@JelleZijlstra_

### Summary

`from _typing import A, B` produces two identical errors:

```
    Cannot resolve import `_typing` (lint:unresolved-import) [Ln 1, Col 6]
    Cannot resolve import `_typing` (lint:unresolved-import) [Ln 1, Col 6]
```

(https://playknot.ruff.rs/dfc4637f-b2da-4f53-86bc-5b2e0bba76d6)

One should be enough.

### Version

_No response_

---

_Comment by @charliermarsh on 2025-05-06 01:48_

👍 Also noticed this in testing the extension.

---

_Label `ty` added by @carljm on 2025-05-06 01:54_

---

_Label `bug` added by @MichaReiser on 2025-05-06 06:40_

---

_Added to milestone `Red Knot Alpha` by @MichaReiser on 2025-05-06 06:40_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-06 10:49_

---

_Closed by @AlexWaygood on 2025-05-06 11:37_

---

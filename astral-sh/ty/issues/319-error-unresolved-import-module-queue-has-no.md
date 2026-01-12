```yaml
number: 319
title: "error[unresolved-import]: Module `queue` has no member `ShutDown` even with Python 3.13"
type: issue
state: closed
author: crazymidnight
labels:
  - question
assignees: []
created_at: 2025-05-11T15:57:06Z
updated_at: 2025-05-12T09:48:36Z
url: https://github.com/astral-sh/ty/issues/319
synced_at: 2026-01-12T15:54:22Z
```

# error[unresolved-import]: Module `queue` has no member `ShutDown` even with Python 3.13

---

_@crazymidnight_

### Summary

Hi! Command `ty check` triggered on import `from queue import Empty, Queue, ShutDown` with error:
error[unresolved-import]: Module `queue` has no member `ShutDown`.
Exception `ShutDown` was added to queue module in Python 3.13.
Link to playground - https://play.ty.dev/894c47ff-2292-444a-a8a7-8612b45bf76d
Also python version in ty.json is 3.13 but playground runs Python 3.12. Locally I run ty with Python 3.13

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Comment by @MichaReiser on 2025-05-12 06:37_

This seems to be fixed now. At least, I don't see any diagnostics in the playground.

>  Also python version in ty.json is 3.13 but playground runs Python 3.12. Locally I run ty with Python 3.13

Yes, the playground uses pyodide and the stable version only supports 3.12 at the moment

---

_Label `question` added by @MichaReiser on 2025-05-12 06:37_

---

_Comment by @crazymidnight on 2025-05-12 09:48_

Thank you for help! I found that ty was right in this case. I had `requires-python = ">=3.12` in pyproject.toml. After upgrading version to 3.13 the error went away

---

_Closed by @crazymidnight on 2025-05-12 09:48_

---

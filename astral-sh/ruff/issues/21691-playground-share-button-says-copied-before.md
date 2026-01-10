```yaml
number: 21691
title: "playground: Share button says Copied! before actually copying, then fails if you tab away too fast"
type: issue
state: closed
author: injust
labels:
  - playground
  - help wanted
assignees: []
created_at: 2025-11-29T10:24:35Z
updated_at: 2025-12-17T11:16:03Z
url: https://github.com/astral-sh/ruff/issues/21691
synced_at: 2026-01-10T11:10:00Z
```

# playground: Share button says Copied! before actually copying, then fails if you tab away too fast

---

_Issue opened by @injust on 2025-11-29 10:24_

### Summary

The Share button shows "Copied!" before it actually copies.

If you press Share, then tab away as soon as you see "Copied!", it will not copy. The console shows this error:

```
Failed to share playground: NotAllowedError: Failed to execute 'writeText' on 'Clipboard': Document is not focused.
(anonymous) @ index-BGT7xqC_.js:1033
Promise.catch
(anonymous) @ index-BGT7xqC_.js:1033
onClick @ index-BGT7xqC_.js:54
kX @ index-BGT7xqC_.js:49
(anonymous) @ index-BGT7xqC_.js:49
AK @ index-BGT7xqC_.js:49
q3 @ index-BGT7xqC_.js:49
o9 @ index-BGT7xqC_.js:50
kbe @ index-BGT7xqC_.js:50
```

### Version

_No response_

---

_Label `playground` added by @MichaReiser on 2025-11-29 14:47_

---

_Label `help wanted` added by @MichaReiser on 2025-11-29 14:47_

---

_Comment by @mahiro72 on 2025-12-07 12:07_

Hi @MichaReiser , I'd like to work on this issue.

---

_Assigned to @mahiro72 by @MichaReiser on 2025-12-08 09:42_

---

_Closed by @MichaReiser on 2025-12-17 11:16_

---

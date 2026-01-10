```yaml
number: 1082
title: hover and goto targets differ
type: issue
state: closed
author: MatthewMckee4
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-21T15:16:04Z
updated_at: 2025-11-23T03:22:05Z
url: https://github.com/astral-sh/ty/issues/1082
synced_at: 2026-01-10T01:58:59Z
```

# hover and goto targets differ

---

_Issue opened by @MatthewMckee4 on 2025-08-21 15:16_

### Summary

See https://play.ty.dev/aef6cdcc-70b9-41f0-b71d-4ad185c169ed

I am talking about line 3 in main.py

When hovering over foo we get `<module 'foo'>` as expected and when hovering over foo.bar we get `<module 'foo.bar'>` as expected.

But when we goto on foo it takes us to foo/bar.py, and we cannot goto on foo.bar (it does not have a definition it seems).

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-08-21 15:28_

---

_Comment by @MichaReiser on 2025-08-22 06:49_

@Gankra this might be a good one to look into

---

_Label `bug` added by @MichaReiser on 2025-08-22 06:50_

---

_Assigned to @Gankra by @Gankra on 2025-08-22 13:39_

---

_Comment by @Gankra on 2025-08-22 13:42_

Oh that's all kinds of scrambled yeah.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:57_

---

_Comment by @Gankra on 2025-11-23 03:22_

At some point this got fixed, possibly driveby from my last cleanup of goto-imports.

---

_Closed by @Gankra on 2025-11-23 03:22_

---

```yaml
number: 11987
title: "Add support for adding markers to all packages in `uv add`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - good first issue
assignees: []
created_at: 2025-03-05T19:32:28Z
updated_at: 2025-03-11T15:29:38Z
url: https://github.com/astral-sh/uv/issues/11987
synced_at: 2026-01-12T16:00:51Z
```

# Add support for adding markers to all packages in `uv add`

---

_@zanieb_

### Summary

Instead of repeating markers for each `uv add <package>`, add an option to allow propagating a marker to all packages.

This is important for migration from `requirements.txt` files which are single platform. In that case, the markers are not recorded in the file.

### Example

```
uv add -r requirements.win.txt --marker "sys_platform == 'win32'"
```

---

_Label `enhancement` added by @zanieb on 2025-03-05 19:32_

---

_Label `good first issue` added by @zanieb on 2025-03-05 19:36_

---

_Comment by @thejchap on 2025-03-06 18:03_

@zanieb took a quick stab at this one, lmk if this is on the right track https://github.com/astral-sh/uv/pull/12012

---

_Closed by @konstin on 2025-03-11 15:29_

---

_Closed by @konstin on 2025-03-11 15:29_

---

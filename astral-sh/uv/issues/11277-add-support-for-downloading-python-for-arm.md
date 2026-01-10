---
number: 11277
title: Add support for downloading Python for ARM Windows11
type: issue
state: closed
author: stefansimik
labels:
  - enhancement
  - external
assignees: []
created_at: 2025-02-06T09:39:13Z
updated_at: 2025-02-06T17:07:00Z
url: https://github.com/astral-sh/uv/issues/11277
synced_at: 2026-01-10T01:25:03Z
---

# Add support for downloading Python for ARM Windows11

---

_Issue opened by @stefansimik on 2025-02-06 09:39_

### Summary

When using Windows virtual machine on MacOS, we have installed Windows 11 for ARM. (for example if using ParallelsDesktop VM)

For this architecture, there is no available Python build, what causes error:

For this command: 
`uv python install 3.12 --verbose`

We got this error:
```
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
error: No download found for request: cpython-3.12-windows-aarch64-none
```

That means, there is no build for `windows` and `aarch64`.

The point here is to add this architecture to the supported downloadable python versions.
See here:

![Image](https://github.com/user-attachments/assets/f77df8b8-3618-46ac-81d6-3e9ee19601ec)

---

_Label `enhancement` added by @stefansimik on 2025-02-06 09:39_

---

_Comment by @charliermarsh on 2025-02-06 14:00_

This needs to be solved in python-build-standalone first! You can track it here: https://github.com/astral-sh/python-build-standalone/issues/386.

---

_Closed by @charliermarsh on 2025-02-06 14:00_

---

_Closed by @charliermarsh on 2025-02-06 14:00_

---

_Label `upstream` added by @charliermarsh on 2025-02-06 14:00_

---

_Comment by @stefansimik on 2025-02-06 17:06_

Clear, thank you for explanation 🙏 

---

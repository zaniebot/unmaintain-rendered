---
number: 8346
title: Add a way to add a file to a build
type: issue
state: closed
author: ebits21
labels: []
assignees: []
created_at: 2024-10-18T19:05:56Z
updated_at: 2024-10-18T19:20:38Z
url: https://github.com/astral-sh/uv/issues/8346
synced_at: 2026-01-10T01:24:27Z
---

# Add a way to add a file to a build

---

_Issue opened by @ebits21 on 2024-10-18 19:05_

Right now I've been able to do this by adding an include in a MANIFEST.in file.

I'm wondering if there could be a setting in pyproject.toml or an uv command to do this.

Sorry if I missed something if this is already possible.

---

_Comment by @zanieb on 2024-10-18 19:08_

Unfortunately this is build-backend specific, i.e. that file is [setuptools-specific](https://setuptools.pypa.io/en/latest/userguide/miscellaneous.html). As a a build frontend, we can't easily provide a way to do this across various build backends. When we have our own build backend (which is in progress), we'll be able to provide this if you're using our backend.

---

_Comment by @ebits21 on 2024-10-18 19:20_

Ah I understand. Thanks!

---

_Closed by @ebits21 on 2024-10-18 19:20_

---

_Referenced in [astral-sh/uv#8383](../../astral-sh/uv/issues/8383.md) on 2024-10-20 19:06_

---

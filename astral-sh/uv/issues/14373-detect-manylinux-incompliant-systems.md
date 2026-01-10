---
number: 14373
title: Detect manylinux-incompliant systems
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-06-30T14:30:49Z
updated_at: 2025-06-30T14:30:58Z
url: https://github.com/astral-sh/uv/issues/14373
synced_at: 2026-01-10T01:25:44Z
---

# Detect manylinux-incompliant systems

---

_Issue opened by @zanieb on 2025-06-30 14:30_

We could detect the fact that a system (such as NixOS) is not manylinux-compliant (e.g. that there's no libstdc++.so.6 on the default search path) and do something with that knowledge. For example, we could refuse to download manylinux wheels or print a warning or error to the user.

---

_Comment by @zanieb on 2025-06-30 14:30_

cc @geofft 

---

_Label `enhancement` added by @zanieb on 2025-06-30 14:30_

---

_Label `wish` added by @zanieb on 2025-06-30 14:30_

---

---
number: 7705
title: "Add `uv build --all` option"
type: issue
state: closed
author: inbalzelig-ugi
labels:
  - enhancement
assignees: []
created_at: 2024-09-26T11:14:09Z
updated_at: 2024-09-27T02:46:08Z
url: https://github.com/astral-sh/uv/issues/7705
synced_at: 2026-01-10T01:24:18Z
---

# Add `uv build --all` option

---

_Issue opened by @inbalzelig-ugi on 2024-09-26 11:14_

Hi, I want to move from rye to uv. In rye, there is a `rye build --all ` option to build all members in a workspace without passing the members' names one by one. 
Current options in uv are:
`uv build` - build the workspace as one package and not split into separate packages for the workspace members.
`uv build --package <member name>` - build just one member, need to run multiple times for each member.

Also, it will be great to have a way to list the workspace members.


---

_Label `enhancement` added by @konstin on 2024-09-26 11:14_

---

_Referenced in [astral-sh/uv#7724](../../astral-sh/uv/pulls/7724.md) on 2024-09-26 19:24_

---

_Closed by @charliermarsh on 2024-09-27 02:46_

---

---
number: 13702
title: "Hint at `tool.uv.required-environments` when installation fails for a package with wheels"
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-05-28T14:13:22Z
updated_at: 2025-09-25T13:54:23Z
url: https://github.com/astral-sh/uv/issues/13702
synced_at: 2026-01-10T01:25:36Z
---

# Hint at `tool.uv.required-environments` when installation fails for a package with wheels

---

_Issue opened by @konstin on 2025-05-28 14:13_

A common user question is that their installation fails after a successful resolution. An example is installing ML packages that in recent versions only support Apple Silicon on an Intel Mac.

For cases where we failed to install a package that has wheels, either because there is no sdist or because there is a not otherwise handled build error, we should recommend `tool.uv.required-environments`.

See https://github.com/astral-sh/uv/issues?q=is%3Aissue%20required-environments and https://discord.com/channels/1039017663004942429/1377300213663793252/1377300213663793252 for examples

---

_Label `error messages` added by @konstin on 2025-05-28 14:13_

---

_Assigned to @Gankra by @Gankra on 2025-06-27 16:23_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-07-28 14:45_

---

_Unassigned @Gankra by @jtfmumm on 2025-07-28 14:45_

---

_Unassigned @jtfmumm by @konstin on 2025-09-12 13:37_

---

_Comment by @konstin on 2025-09-25 13:54_

Fixed in https://github.com/astral-sh/uv/pull/13575

---

_Closed by @konstin on 2025-09-25 13:54_

---

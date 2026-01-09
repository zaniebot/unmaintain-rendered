---
number: 9871
title: "Add `uv self uninstall` command"
type: issue
state: open
author: alexklavensnyt
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-12-13T16:05:18Z
updated_at: 2025-02-19T07:35:30Z
url: https://github.com/astral-sh/uv/issues/9871
synced_at: 2026-01-07T13:12:18-06:00
---

# Add `uv self uninstall` command

---

_Issue opened by @alexklavensnyt on 2024-12-13 16:05_

Uninstalling uv feels a bit hacky.

Two suggested improvements:

1. An official uninstallation script (similar to the standalone installer), or `uv self uninstall`. 
2. If neither are realistic, improve the documentation for the manual uninstall. The current instructions first mention removing the binaries, and then mention a suggestion for removing stored data. The recommended data removal process involves uv commands that will no longer be available if the binaries are gone 😄. 

Thanks!

---

_Referenced in [astral-sh/uv#9938](../../astral-sh/uv/pulls/9938.md) on 2024-12-16 15:17_

---

_Renamed from "Clarify or improve uninstallation process" to "Add `uv self uninstall` command" by @zanieb on 2025-01-06 20:53_

---

_Label `help wanted` added by @zanieb on 2025-01-06 20:53_

---

_Label `cli` added by @zanieb on 2025-01-06 20:53_

---

_Comment by @zanieb on 2025-01-06 20:53_

This should be pretty easy, if someone wants to take on the command. The documentation was improved in #9938 

---

_Comment by @zanieb on 2025-01-06 20:54_

We'll probably want a flag to keep / remove data.

---

_Referenced in [astral-sh/uv#11456](../../astral-sh/uv/issues/11456.md) on 2025-02-12 21:13_

---

_Comment by @zmievsa on 2025-02-16 19:18_

I will take this if everyone's OK with it

---

_Comment by @zanieb on 2025-02-16 19:22_

@zmievsa Go for it!

---

_Referenced in [astral-sh/uv#11613](../../astral-sh/uv/pulls/11613.md) on 2025-02-19 03:16_

---

_Comment by @loic-lescoat on 2025-02-19 07:33_

@zmievsa I am mostly done with implementing this in https://github.com/astral-sh/uv/pull/11613; would you like to contribute there?

---

_Comment by @zmievsa on 2025-02-19 07:35_

@loic-lescoat no worries, you can just proceed with your implementation. I'm happy as long as problem is solved :)

---

_Referenced in [astral-sh/uv#13972](../../astral-sh/uv/issues/13972.md) on 2025-06-11 17:37_

---

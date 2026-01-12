```yaml
number: 12628
title: "Report the queried executable path in `uv python list`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: zb/shim-fix
created_at: 2025-04-02T16:05:33Z
updated_at: 2025-04-02T20:45:39Z
url: https://github.com/astral-sh/uv/pull/12628
synced_at: 2026-01-12T16:10:20Z
```

# Report the queried executable path in `uv python list`

---

_@zanieb_

In an attempt to avoid reporting shims as their resolved `sys.executable` path, we report the queried executable path instead. This seems more correct for this command, broadly? Interestingly, it changes the reported paths for Homebrew Python

<img width="1430" alt="Screenshot 2025-04-02 at 11 05 18 AM" src="https://github.com/user-attachments/assets/0e1600e8-fb07-40c7-a6d6-56eaeb4b9293" />

Closes #9979 

---

_Label `cli` added by @zanieb on 2025-04-02 16:05_

---

_Review requested from @Gankra by @zanieb on 2025-04-02 16:08_

---

_Comment by @zanieb on 2025-04-02 16:09_

I can try to add some test coverage here, but it seems kind of tricky.

---

_Comment by @zanieb on 2025-04-02 16:10_

@Gankra would you try this out on your bespoke Windows setup?

---

_Comment by @Gankra on 2025-04-02 17:25_

Confirmed this is working the way you want:

```
cpython-3.13.2-windows-x86_64-none                   C:\ProgramData\chocolatey\bin\python3.13.exe
cpython-3.13.2-windows-x86_64-none                   C:\Users\gankra\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe
cpython-3.12.9-windows-x86_64-none                   <download available>
cpython-3.11.11-windows-x86_64-none                  C:\Users\gankra\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe
```

---

_Marked ready for review by @zanieb on 2025-04-02 17:52_

---

_@Gankra approved on 2025-04-02 19:16_

---

_Comment by @Gankra on 2025-04-02 19:16_

Hmm wait does this change whether we actually invoke the shim? The "bug" was purely display?

---

_Comment by @zanieb on 2025-04-02 19:24_

Can you say more? Interpreter discovery invokes each executable it finds. Previously, we reported `sys.executable` (which for a shim, is the resolved path) — now, we report the invoked executable (i.e., the path we discovered).

---

_Label `enhancement` added by @zanieb on 2025-04-02 19:24_

---

_Merged by @zanieb on 2025-04-02 19:24_

---

_Closed by @zanieb on 2025-04-02 19:24_

---

_Branch deleted on 2025-04-02 19:25_

---

_Comment by @Gankra on 2025-04-02 20:41_

The detail I was unclear on was whether we *kept* invoking through that executable once we found it, or if we cached the sys.executable path and used that directly.

---

_Comment by @zanieb on 2025-04-02 20:45_

Well.. we keep invoking through `sys.executable` in general, because we never tracked the original path. In `uv python list`, we don't invoke the interpreter again.

---

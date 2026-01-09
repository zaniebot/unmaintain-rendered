---
number: 9151
title: "Infer `.cmd` and `.ps1` extensions during `uv run`"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - windows
assignees: []
created_at: 2024-11-15T14:44:14Z
updated_at: 2025-03-10T11:09:41Z
url: https://github.com/astral-sh/uv/issues/9151
synced_at: 2026-01-07T13:12:18-06:00
---

# Infer `.cmd` and `.ps1` extensions during `uv run`

---

_Issue opened by @zanieb on 2024-11-15 14:44_

As discussed in https://github.com/astral-sh/uv/issues/8770

If the user provides `uv run foo` and `foo.cmd` or `foo.ps1` exists we should invoke it unless `foo.exe` exists.

This is a little questionable, i.e., should we apply this to everything in the `PATH`? Or just the virtual environment `Scripts` directory?

---

_Referenced in [astral-sh/uv#8770](../../astral-sh/uv/issues/8770.md) on 2024-11-15 14:44_

---

_Label `windows` added by @zanieb on 2024-11-15 14:46_

---

_Comment by @samypr100 on 2024-11-19 00:13_

> Or just the virtual environment Scripts directory?

I'd expect this behavior at least from a security perspective.

Also see [note](https://doc.rust-lang.org/std/process/index.html#batch-file-special-handling) in the special handling of cmd.exe and .bat

> running batch scripts in this way may be removed in the future and so should not be relied upon.

---

_Referenced in [astral-sh/uv#9303](../../astral-sh/uv/issues/9303.md) on 2024-11-21 04:26_

---

_Referenced in [Nuitka/Nuitka#3173](../../Nuitka/Nuitka/issues/3173.md) on 2024-11-26 12:49_

---

_Comment by @kdeldycke on 2024-11-27 08:06_

Also, see the list of default extensions recognised for Windows executables in Python standard library:

https://github.com/python/cpython/blob/6d3b5206cfaf5a85c128b671b1d9527ed553c930/Lib/shutil.py#L55

---

_Comment by @kdeldycke on 2025-02-03 05:03_

For the record I just updated my test case with latest `uv` (0.5.26) and `nuitka` (2.6.2) to confirm the issue is still reproducible: https://github.com/kdeldycke/nuitka-issue-3173/actions/runs/13106670972/job/36562560546

---

_Label `help wanted` added by @zanieb on 2025-02-03 13:51_

---

_Comment by @kdeldycke on 2025-02-25 08:56_

Just to keep track of the progress, the issue can still be reproduced with the `uv 0.6.3` and `nuitka 2.6.7` combo: https://github.com/kdeldycke/nuitka-issue-3173/actions/runs/13517137348

---

_Referenced in [astral-sh/uv#11888](../../astral-sh/uv/pulls/11888.md) on 2025-03-02 00:33_

---

_Closed by @zanieb on 2025-03-05 20:39_

---

_Closed by @zanieb on 2025-03-05 20:39_

---

_Comment by @kdeldycke on 2025-03-10 11:09_

I confirm the issue has been fixed in uv 0.6.5, see: https://github.com/Nuitka/Nuitka/issues/3173#issuecomment-2710215547

---

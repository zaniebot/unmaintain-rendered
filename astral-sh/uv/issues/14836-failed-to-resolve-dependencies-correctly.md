---
number: 14836
title: Failed to resolve dependencies correctly
type: issue
state: closed
author: jalenzz
labels:
  - bug
  - resolver
assignees: []
created_at: 2025-07-23T06:40:24Z
updated_at: 2025-08-12T02:30:47Z
url: https://github.com/astral-sh/uv/issues/14836
synced_at: 2026-01-10T01:25:49Z
---

# Failed to resolve dependencies correctly

---

_Issue opened by @jalenzz on 2025-07-23 06:40_

### Summary

I try adding torch to a totally new venv, but got error (with `--no-cache` get the same error).

```
$ uv add torch          
Resolved 48 packages in 28ms
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.8 (`cp38`), but `torch` (v2.5.1) only has wheels with the following Python implementation tags: `cp39`, `cp310`, `cp311`, `cp312`, `cp313`
```

Why isn't uv looking for a torch version compatible with py3.8?

If I use `uv pip install torch`, uv correctly installed the compatible version of torch.

### Platform

ubuntu 20.04

### Version

uv 0.8.0

### Python version

Python 3.8.20

---

_Label `bug` added by @jalenzz on 2025-07-23 06:40_

---

_Comment by @zanieb on 2025-07-23 12:52_

I presume you have a `requires-python` with a lower bound of 3.8?

---

_Label `bug` removed by @zanieb on 2025-07-23 12:52_

---

_Label `question` added by @zanieb on 2025-07-23 12:52_

---

_Comment by @jalenzz on 2025-07-24 01:41_

@zanieb Thanks for you reply. Yes `requires-python = ">=3.8"`
But 3.8 should support `torch==2.4.1`, I wonder why not following this version.

---

_Comment by @zanieb on 2025-07-24 02:21_

Can you provide verbose logs? (`-v`)

---

_Comment by @zanieb on 2025-07-24 02:23_

And a complete reproduction? e.g., starting from `uv init example`?

---

_Comment by @jalenzz on 2025-07-24 05:07_

```
➜  ~ uv init example -p 3.8 && cd example
Initialized project `example` at `/home/xxx/example`
➜  example git:(master) ✗ uv add torch -v                     

error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.8 (`cp38`), but `torch` (v2.5.1) only has wheels with the following Python implementation tags: `cp39`, `cp310`, `cp311`, `cp312`, `cp313`
```

full verbose [log](https://github.com/user-attachments/files/21400339/log.txt)

thanks :>

---

_Comment by @zanieb on 2025-07-26 02:55_

I can reproduce

```
❯ uv init example -p 3.8 && cd example
Initialized project `example` at `/private/tmp/example`
❯ uv add torch
Using CPython 3.8.20
Creating virtual environment at: .venv
Resolved 48 packages in 1.16s
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.8 (`cp38`), but `torch` (v2.5.1) only has wheels with the following Python implementation tags: `cp39`, `cp310`, `cp311`, `cp312`, `cp313`
```

Here's the lockfile https://gist.github.com/zanieb/69a4cc50b632f01884095465d631ce24

If I grab a torch wheel and unpack it... it claims to support 3.8

```
❯ cat torch-2.5.1.dist-info/METADATA | grep Requires-Python
Requires-Python: >=3.8.0
```

I presume that's part of the problem? This behavior seems wrong though, we shouldn't select that version Python 3.8 without a usable wheel.

---

_Label `question` removed by @zanieb on 2025-07-26 02:56_

---

_Label `bug` added by @zanieb on 2025-07-26 02:56_

---

_Label `resolver` added by @zanieb on 2025-07-26 02:56_

---

_Assigned to @zanieb by @zanieb on 2025-07-26 02:57_

---

_Referenced in [astral-sh/uv#14913](../../astral-sh/uv/pulls/14913.md) on 2025-07-26 04:10_

---

_Comment by @jalenzz on 2025-07-26 18:40_

Thanks!
Looking forward to the PR merge

---

_Closed by @jalenzz on 2025-08-12 01:33_

---

_Comment by @zanieb on 2025-08-12 02:30_

I do intend to make that the default behavior at some point too, so you don't need to set required environments.

---

_Referenced in [astral-sh/uv#15547](../../astral-sh/uv/issues/15547.md) on 2025-08-27 15:55_

---

_Referenced in [astral-sh/uv#15932](../../astral-sh/uv/pulls/15932.md) on 2025-09-18 14:04_

---

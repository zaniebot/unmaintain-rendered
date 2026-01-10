---
number: 15699
title: Support for shared objects
type: issue
state: closed
author: pswsm
labels:
  - question
assignees: []
created_at: 2025-09-05T10:08:37Z
updated_at: 2025-09-16T23:00:05Z
url: https://github.com/astral-sh/uv/issues/15699
synced_at: 2026-01-10T01:25:58Z
---

# Support for shared objects

---

_Issue opened by @pswsm on 2025-09-05 10:08_

### Question

I'm working on a project where a needed library is supplied in the form of a shared object (ex: `sdkPy.cpython.so`).

This is usually not a problem, since we can import it normally with python. However, when using `uv` to manage the virtual environment and then running it with `uv run` it's unable to find this library.

Is there any way to add them as local source or manual imports of some sort so the interpreter can actually find them? Should it be in a specific folder? Currently it's sitting alognside the file that imports it (say `src/[libname]/wrap/sdkPy.cpython.so`)

### Platform

fedora 6.16.3-200.fc42.x86_64

### Version

uv 0.8.8

---

_Label `question` added by @pswsm on 2025-09-05 10:08_

---

_Comment by @konstin on 2025-09-05 12:15_

The best practice here is to build the share library into a wheel and install that wheel into a project, is there a reason why you're using the raw library?

For debugging import problems, I'd start with looking at `sys.path`.

---

_Comment by @pswsm on 2025-09-16 23:00_

Hi!

Sorry for the lateness. The reason I'm using the raw library is because the 3rd party provided it like that, and at my team we can't spare much time into building a wrapper around it.

This library references another local library (also provided by 3rd party), which we believe is the actual C library.

We got it working by using `importlib`.

Thanks!

---

_Closed by @pswsm on 2025-09-16 23:00_

---

---
number: 15771
title: "Is it possible to install a version of python that includes the \"test\" package?"
type: issue
state: open
author: JanEricNitschke
labels:
  - question
assignees: []
created_at: 2025-09-10T17:56:22Z
updated_at: 2025-09-10T18:58:13Z
url: https://github.com/astral-sh/uv/issues/15771
synced_at: 2026-01-07T13:12:19-06:00
---

# Is it possible to install a version of python that includes the "test" package?

---

_Issue opened by @JanEricNitschke on 2025-09-10 17:56_

### Question

I was thinking of taking a shot at this typing_extension issue: https://github.com/python/typing_extensions/issues/606 and wanted to test out the linked CI locally to check what can be done to fix the remaining issues.

However locally (with uv or any other standard way that isnt manually build CPython from source) i do not get the [test module](https://docs.python.org/3/library/test.html) which is needed: https://github.com/python/cpython/blob/3.14/Lib/test/test_typing.py#L52

Interestingly the github action that sets up python seems to include it.

I was wondering whether uv also somehow offers the possibility to include when installing python or could do so in the future.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @JanEricNitschke on 2025-09-10 17:56_

---

_Comment by @konstin on 2025-09-10 18:04_

CC @AlexWaygood 

---

_Comment by @zanieb on 2025-09-10 18:50_

We exclude this module in our `install-only` builds per

https://github.com/astral-sh/python-build-standalone/blob/0fa18acc1e60c981c7c315255bc3bbe4bf1f16fa/src/release.rs#L405-L419

but I think it's probably present in our full distributions attached to the `python-build-standalone` releases?

There's not a way to select those in uv today.

---

_Comment by @AlexWaygood on 2025-09-10 18:58_

> However locally (with uv or any other standard way that isnt manually build CPython from source) i do not get the [test module](https://docs.python.org/3/library/test.html) which is needed: [python/cpython@`3.14`/Lib/test/test_typing.py#L52](https://github.com/python/cpython/blob/3.14/Lib/test/test_typing.py?rgh-link-date=2025-09-10T17%3A56%3A22.000Z#L52)

The `test` module is included in the binaries uploaded to python.org, but yeah, it's often excluded in other distributions of Python (PBS is definitely not an outlier here). The reason is that it's really only intended to be useful for CPython developers; it's not meant to be a user-facing module and has no "public API". But you would need to find a build of Python with it available to fix that typing_extensions issue, yes.

Off the top of my head, it's also excluded in Conda builds and I think several Linux distro builds. I can't remember whether pyenv includes it or not.

---

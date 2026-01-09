---
number: 11387
title: Allow request and installation of Python interpreters with debug symbols
type: issue
state: open
author: willnewton
labels:
  - enhancement
assignees: []
created_at: 2025-02-10T15:47:49Z
updated_at: 2025-02-11T20:33:30Z
url: https://github.com/astral-sh/uv/issues/11387
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow request and installation of Python interpreters with debug symbols

---

_Issue opened by @willnewton on 2025-02-10 15:47_

### Question

For the purposes of being able to attach a debugger to Python processes running in uv managed interpreters I would like to be able to get an interpreter with debug symbols. However the interpreter that uv installs by default doesn't have them.

This PR seems to suggest that there was going to be a --debug flag added that might have accomplished this but it doesn't look like that happened as far as I can tell?

https://github.com/astral-sh/uv/pull/5451

Thanks,

### Platform

macOS, Linux

### Version

uv 0.5.20

---

_Label `question` added by @willnewton on 2025-02-10 15:47_

---

_Comment by @zanieb on 2025-02-10 16:06_

I think there's not a way to request these yet, though I forged the way with the free-threaded variant handling.

---

_Renamed from "Is it possible to install a python interpreter with debugging symbols?" to "Allow request and installation of Python interpreters with debug symbols" by @zanieb on 2025-02-10 16:07_

---

_Label `question` removed by @zanieb on 2025-02-10 16:07_

---

_Label `enhancement` added by @zanieb on 2025-02-10 16:07_

---

_Comment by @zanieb on 2025-02-10 16:08_

Related #8100 / https://github.com/astral-sh/uv/pull/8100/files#r1795793108

---

_Comment by @geofft on 2025-02-11 19:28_

If you just want debug _symbols_ instead of a separate build with debugging enabled, those are available from the full releases at https://github.com/astral-sh/python-build-standalone/releases . `uv` installs the stripped install-only archives, but unstripped archives are available too.

We can certainly do something to automate the process of downloading the right version (or maybe we should run a debuginfod server or something), but I figured I'd mention it in case you have a core file or something and want a symbolicated backtrace—you shouldn't need to recreate the problem on a separate build, you should be able to point your debugger at one of the full releases.

---

_Referenced in [astral-sh/python-build-standalone#522](../../astral-sh/python-build-standalone/issues/522.md) on 2025-02-11 21:14_

---

_Referenced in [astral-sh/uv#11518](../../astral-sh/uv/issues/11518.md) on 2025-02-14 19:46_

---

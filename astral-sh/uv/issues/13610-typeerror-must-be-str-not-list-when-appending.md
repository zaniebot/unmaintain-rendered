---
number: 13610
title: "`TypeError: must be str, not list` when appending subtype of `str` to a `list` in uv-managed python installation on linux"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2025-05-23T03:43:28Z
updated_at: 2025-06-01T01:23:48Z
url: https://github.com/astral-sh/uv/issues/13610
synced_at: 2026-01-10T01:25:35Z
---

# `TypeError: must be str, not list` when appending subtype of `str` to a `list` in uv-managed python installation on linux

---

_Issue opened by @DetachHead on 2025-05-23 03:43_

### Summary

this is a very strange bug because the following code only seems to crash on an uv-managed python installation and only on linux:

```py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
```
```
$ uv run python3.13
Python 3.13.3 (main, May 22 2025, 02:00:23) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo(str): ...
...
... a = []
... new_value = Foo("1")
... a += new_value
...
Traceback (most recent call last):
  File "<python-input-0>", line 5, in <module>
    a += new_value
TypeError: must be str, not list
```

on the system version of python it works fine:

```
$ python3.13
Python 3.13.3 (main, Apr  9 2025, 08:55:03) [GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo(str): ...
...
... a = []
... new_value = Foo("1")
... a += new_value
...
>>>
```

but on windows it works fine even, with the uv-managed python version:

```
> uv run python
Python 3.13.3 (main, Apr  9 2025, 04:04:49) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo(str): ...
... 
>>> a = []
>>> new_value = Foo("1")
>>> a += new_value
>>>
```

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

0.7.7

### Python version

3.13.3

---

_Label `bug` added by @DetachHead on 2025-05-23 03:43_

---

_Comment by @harshil21 on 2025-05-23 05:15_

Can confirm. If you use the python packaged with uv 0.7.5, it works fine:

```
uvx uv@0.7.5 python install --reinstall
Installed 2 versions in 2.46s
 ~ cpython-3.10.17-linux-x86_64-gnu
 ~ cpython-3.13.3-linux-x86_64-gnu
```

```
uv run python                          
Python 3.13.3 (main, Apr  9 2025, 04:03:52) [Clang 20.1.0 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo(str): ...
... 
>>> a = []
>>> new_value = Foo("1")
>>> a += new_value
```

---

_Assigned to @geofft by @geofft on 2025-05-23 09:49_

---

_Comment by @zanieb on 2025-05-23 13:48_

Here are some reproductions....

Working:

```dockerfile
FROM --platform=linux/amd64 ghcr.io/astral-sh/uv:0.7.5-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.13 mre.py
```

Failing:

```dockerfile
FROM --platform=linux/amd64 ghcr.io/astral-sh/uv:0.7.6-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.13 mre.py
```

With error:

```
Traceback (most recent call last):
      File "//mre.py", line 5, in <module>
        a += new_value
    TypeError: must be str, not list
```

The python-build-standalone distribution changes between these releases are at
https://github.com/astral-sh/python-build-standalone/compare/20250409...20250517

It seems the regression is Linux-only, which makes me suspicious of
https://github.com/astral-sh/python-build-standalone/pull/592 since we are not applying that on
macOS.

Note this also fails on uv v0.7.7 (the latest version).

This appears to be x86-64 specific, e.g., the following succeeds (on aarch64):

```dockerfile
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:0.7.6-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.13 mre.py
```

---

_Comment by @zanieb on 2025-05-23 14:02_

This does not reproduce on 3.14 aarch64

```
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:0.7.6-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.14 mre.py
```

---

_Comment by @zanieb on 2025-05-23 14:05_

It does reproduce on 3.12 x86-64

```
FROM --platform=linux/amd64 ghcr.io/astral-sh/uv:0.7.6-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.12 mre.py
```

---

_Comment by @zanieb on 2025-05-23 14:05_

Okay, and it reproduces on 3.14 x86-64

```
FROM --platform=linux/amd64 ghcr.io/astral-sh/uv:0.7.6-debian-slim

COPY <<EOF /mre.py
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
EOF

RUN uv run -p 3.14 mre.py
```

---

_Referenced in [astral-sh/uv#13617](../../astral-sh/uv/pulls/13617.md) on 2025-05-23 14:11_

---

_Referenced in [astral-sh/python-build-standalone#622](../../astral-sh/python-build-standalone/pulls/622.md) on 2025-05-23 17:46_

---

_Closed by @Gankra on 2025-05-23 22:15_

---

_Comment by @Gankra on 2025-05-23 23:00_


# Mitigations

`uv 0.7.8` is mostly reverting python-build-standalone from 202505017 to 20250409 (the one used in `uv 0.7.5`).

In particular we have reverted it on non-aarch64 Linux, and also completely removed `Python 3.14.0a7` to avoid portability issues between platforms. The net effect is the following changes have been defacto reverted:

- Add Python 3.14 on musl
- free-threaded Python on musl
- Add Python 3.14.0a7
- Statically link `libpython` into the interpreter on Linux for a significant performance boost



# Future Work

We believe we have found the issue with our Python builds, and are working on testing and releasing the fix (but it's friday night on a long weekend, so we'd rather not rush out a hotfix): 

* https://github.com/astral-sh/python-build-standalone/pull/622



# Problem

On affected platforms, running the following python program with a uv-provided python would result in spurious type errors about string:

```python
class Foo(str): ...

a = []
new_value = Foo("1")
a += new_value
```

```
Traceback (most recent call last):
  File "<python-input-0>", line 5, in <module>
    a += new_value
TypeError: must be str, not list
```


# Affected Platforms

Here is the testing I've seen (❌ has bug, ✅ works):

* uv version (pbs version)
  * ✅ uv 0.7.8 (pbs 20250409)
  * ❌ uv 0.7.7 (pbs 20250521)
  * ❌ uv 0.7.6 (pbs 20250517)
  * ✅ uv 0.7.5 (pbs 20250409)
* os
  * ❌ linux
  * ✅ macos
  * ✅ windows
* arch
  * ❌ x86_64
  * ✅ aarch64
* python version
  * ❌ 3.12
  * ❌ 3.13
  * ❌ 3.14

Based on this testing it seemed clear that the issue was isolated to linux, and it's *probably* x64 specific, but I opted to handle that conservatively and just reverted "linux, not-aarch64".

---

_Referenced in [Homebrew/homebrew-core#224582](../../Homebrew/homebrew-core/pulls/224582.md) on 2025-05-24 02:47_

---

_Comment by @geofft on 2025-06-01 01:23_

uv 0.7.9, released yesterday, restores static linking of libpython but has a fix for this bug.

If you are still seeing this, please do comment here (after double-checking that you're on the latest version of things with `uv self update && uv python install --reinstall`).

---

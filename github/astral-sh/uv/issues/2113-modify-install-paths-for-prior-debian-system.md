---
number: 2113
title: Modify install paths for prior Debian system Pythons
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-03-01T15:12:29Z
updated_at: 2024-03-05T21:23:37Z
url: https://github.com/astral-sh/uv/issues/2113
synced_at: 2026-01-07T13:12:17-06:00
---

# Modify install paths for prior Debian system Pythons

---

_Issue opened by @charliermarsh on 2024-03-01 15:12_

Within:

```
# Use bullseye
FROM debian:bullseye

RUN apt-get update
RUN apt-get install python3 python3-pip python3-venv -y
```

We see:

```
root@ff9ce2004eb7:/# python3
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sysconfig
>>> sysconfig.get_paths()
{'stdlib': '/usr/lib/python3.9', 'platstdlib': '/usr/lib/python3.9', 'purelib': '/usr/lib/python3.9/site-packages', 'platlib': '/usr/lib/python3.9/site-packages', 'include': '/usr/include/python3.9', 'platinclude': '/usr/include/python3.9', 'scripts': '/usr/bin', 'data': '/usr'}
>>> import sys
>>> sys.path
['', '/usr/lib/python39.zip', '/usr/lib/python3.9', '/usr/lib/python3.9/lib-dynload', '/usr/local/lib/python3.9/dist-packages', '/usr/lib/python3/dist-packages']
```

So when installing into Debian's system Python, we'll end up installing into the "wrong" place. See Filipe's excellent article on it here: https://ffy00.github.io/blog/02-python-debian-and-the-install-locations/.

(I don't see this issue with latest Debian.)

---

_Label `bug` added by @charliermarsh on 2024-03-01 15:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-01 15:12_

---

_Comment by @charliermarsh on 2024-03-02 03:55_

This is really complex, we'd need to vendor quite a lot of Python code from `pip`. It also seems like `disutils` needs a distribution name in order to generate paths, but we want to generate these without such a name. Maybe it's possible? Will play around with it.

---

_Comment by @charliermarsh on 2024-03-02 13:47_

Note that this only affects installing with `--system`.

---

_Comment by @charliermarsh on 2024-03-02 13:47_

> It also seems like `disutils` needs a distribution name in order to generate paths, but we want to generate these without such a name. Maybe it's possible? Will play around with it.

Yes, possible, `virtualenv` does it without providing a dist name.

---

_Referenced in [astral-sh/uv#2131](../../astral-sh/uv/pulls/2131.md) on 2024-03-02 15:00_

---

_Comment by @charliermarsh on 2024-03-02 15:04_

The thing we want to do here is:

- Vendor pip's `locations.get_scheme`: https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/locations/__init__.py#L230
- This requires vendoring pip's `_sysconfig.get_scheme`: https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/locations/_sysconfig.py#L124.
- This also requires vendoring pip's `_distutils.get_scheme`: https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/locations/_distutils.py#L115.

Add all of this to `get_interpreter_info.py`, and change our internal structs to use the `Scheme` schema rather than `SysconfigPaths` (i.e., a subset). However, instead of `headers`, we should use `include`, and omit the distribution name from the end, so that it's generic across distributions. (It _should_ (?) be okay to omit the distribution name when calling `distutils`, virtualenv omits it, although I'm not 100% sure.)

You can also see an example of how virtualenv does this: https://github.com/pypa/virtualenv/blob/5cd543fdf8047600ff2737babec4a635ad74d169/src/virtualenv/discovery/py_info.py#L80.

---

_Referenced in [astral-sh/uv#2193](../../astral-sh/uv/pulls/2193.md) on 2024-03-05 02:07_

---

_Closed by @charliermarsh on 2024-03-05 21:23_

---

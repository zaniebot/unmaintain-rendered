```yaml
number: 12459
title: how to run setcap on uv provided python executable
type: issue
state: closed
author: jsiverskog
labels:
  - question
assignees: []
created_at: 2025-03-25T10:06:35Z
updated_at: 2025-04-03T13:51:29Z
url: https://github.com/astral-sh/uv/issues/12459
synced_at: 2026-01-12T16:01:03Z
```

# how to run setcap on uv provided python executable

---

_@jsiverskog_

### Question

we're having some special requirements. we're using the [pysoem](https://github.com/bnjmnp/pysoem) library to communicate with ethercat devices. this means that the python executable needs `cap_net_raw+ep` capability, which can be achieved by running `sudo setcap cap_net_raw+ep /path/to/python`.

this works fine with the system python, but when setting it on the `uv` provided python executable:
```
sudo setcap cap_net_raw+ep ~/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
```
i get this when i then try to run python:
```
[...]/python3: error while loading shared libraries: $ORIGIN/../lib/libpython3.11.so.1.0: DST not allowed in SUID/SGID programs
```
i'm not sure how the uv built python differs from the system provided one (that could affect this)?

### Platform

pop!_os 22.04 lts (based on ubuntu 24.04) amd64

### Version

uv 0.6.8

---

_Label `question` added by @jsiverskog on 2025-03-25 10:06_

---

_Comment by @jsiverskog on 2025-03-25 10:27_

if i run this:
`patchelf --replace-needed "\$ORIGIN/../lib/libpython3.11.so.1.0" ~/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/lib/libpython3.11.so.1.0  ~/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11`

it works, so it seems to be related to the relative path. and sure, from a security point of view i can understand why that may be problematic.

is there a better way to achieve what we need?

---

_Comment by @jsiverskog on 2025-03-25 12:25_

found this: https://github.com/astral-sh/python-build-standalone/blob/f0abfc9cb1f6a985fc5561cf5435f7f6e8a64e5b/cpython-unix/build-cpython.sh#L684-L697

i'm not sure what the right way forward is here. perhaps the issue should be moved to the `python-build-standalone` repository.

---

_Comment by @jsiverskog on 2025-04-03 13:51_

closing this, opened an issue in python-build-standalone instead: https://github.com/astral-sh/python-build-standalone/issues/576

---

_Closed by @jsiverskog on 2025-04-03 13:51_

---

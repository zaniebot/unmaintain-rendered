---
number: 15571
title: Tkinter app fails with uvs python 3.13
type: issue
state: closed
author: vchalmel
labels:
  - bug
assignees: []
created_at: 2025-08-28T16:38:20Z
updated_at: 2025-11-26T22:15:15Z
url: https://github.com/astral-sh/uv/issues/15571
synced_at: 2026-01-10T01:25:57Z
---

# Tkinter app fails with uvs python 3.13

---

_Issue opened by @vchalmel on 2025-08-28 16:38_

### Summary

While working on a tkinter app I was unable to launch my app through uv but using the systems python was working

Here is the error message :
```
[xcb] Unknown sequence number while appending request
[xcb] You called XInitThreads, this is not your fault
[xcb] Aborting, sorry about that.
python: ../../src/xcb_io.c:157: append_pending_request: Assertion `!xcb_xlib_unknown_seq_number' failed.
Abandon (core dumped)
```

I spotted an irregularity with ldd
ldd $(which python3)
```
        linux-vdso.so.1 (0x00007fff48094000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x000074659cb38000)
        libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x000074659cb19000)
        libexpat.so.1 => /lib/x86_64-linux-gnu/libexpat.so.1 (0x000074659caee000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x000074659c90d000)
        /lib64/ld-linux-x86-64.so.2 (0x000074659cc30000)
```
while ldd .venv/bin/python was outputting:
```
        linux-vdso.so.1 (0x00007ffd931f2000)
        /home/[.hiding my path.]/.venv/bin/../lib/libpython3.13.so.1.0 => not found
        libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007200acb3b000)
        libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007200acb36000)
        libutil.so.1 => /lib/x86_64-linux-gnu/libutil.so.1 (0x00007200acb31000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007200aca51000)
        librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007200aca4a000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007200ac869000)
        /lib64/ld-linux-x86-64.so.2 (0x00007200acb58000)
```

WSL2 uses wayland not X11 for what it's worth

### Platform

Debian on WSL

### Version

uv 0.8.13

### Python version

Python 3.13

---

_Label `bug` added by @vchalmel on 2025-08-28 16:38_

---

_Comment by @zanieb on 2025-08-28 17:00_

Are you on the latest version of the Python distribution? `uv python upgrade --reinstall`?

---

_Comment by @zanieb on 2025-08-28 17:00_

(we fixed this recently)

---

_Comment by @Jeremiah-England on 2025-11-26 18:56_

Updating from 3.13.4 to 3.13.9 fixed this for me.

---

_Closed by @zanieb on 2025-11-26 22:15_

---

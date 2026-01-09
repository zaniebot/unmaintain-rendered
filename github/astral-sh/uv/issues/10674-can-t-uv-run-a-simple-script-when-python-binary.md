---
number: 10674
title: "Can't `uv run` a simple script when Python binary is a symlink with uv 0.5.x"
type: issue
state: closed
author: gozdal
labels:
  - external
assignees: []
created_at: 2025-01-16T11:37:53Z
updated_at: 2025-01-22T16:49:07Z
url: https://github.com/astral-sh/uv/issues/10674
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't `uv run` a simple script when Python binary is a symlink with uv 0.5.x

---

_Issue opened by @gozdal on 2025-01-16 11:37_

I ran into a bug with Ubuntu 24, which has (by default) system-wide Python 3.12.
Coincidently I have our own Python 3.12 installed, which is symlinked from `/usr/local/bin/python3.12`:
`$ ls -al /usr/bin/python3.12`
```
-rwxr-xr-x 1 root root 8019136 Nov  6 13:32 /usr/bin/python3.12
```
`$ ls -al /usr/local/bin/python3.12`
```
lrwxrwxrwx 1 root root 39 Nov 27 14:02 /usr/local/bin/python3.12 -> /opt/starfish/python3.12/bin/python3.12
```
`$ which python3.12`
```
/usr/local/bin/python3.12
```

Now, if I try to run `example.py`:
```python
# /// script
# requires-python = ">=3.12"
# dependencies = []
# ///

import sys

def main() -> None:
    print(sys.version)


if __name__ == "__main__":
    main()
```

uv 0.4.30 works:
`uv cache clean && ./0.4.30/uv run example.py`
```
Clearing cache at: /home/gozdal/.cache/uv
Removed 47 files (64.9KiB)
Reading inline script metadata from `example.py`
3.12.5 (main, Nov 14 2024, 04:29:17) [GCC 13.2.0]
```

uv 0.5.0 and 0.5.20 (latest as I am writing this) fail with `ModuleNotFoundError`:

`uv cache clean && ./0.5.0/uv run example.py`
```
Clearing cache at: /home/gozdal/.cache/uv
Removed 26 files (35.2KiB)
Reading inline script metadata from `example.py`
error: Querying Python at `/home/gozdal/.cache/uv/archive-v0/FUbvxrhgdDsOgH-FF1as3/bin/python3` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/gozdal/.cache/uv/.tmpXvEd2n/python/get_interpreter_info.py", line 12, in <module>
    import struct
  File "/usr/lib/python3.12/struct.py", line 13, in <module>
    from _struct import *
ModuleNotFoundError: No module named '_struct'
---
```

`uv cache clean && ./0.5.20/uv run example.py`
```
Clearing cache at: /home/gozdal/.cache/uv
Removed 24 files (29.8KiB)
error: Querying Python at `/home/gozdal/.cache/uv/archive-v0/xvAW0xfqtaDWv89Ju96Pb/bin/python3` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/gozdal/.cache/uv/.tmpDMAraa/python/get_interpreter_info.py", line 12, in <module>
    import struct
  File "/usr/lib/python3.12/struct.py", line 13, in <module>
    from _struct import *
ModuleNotFoundError: No module named '_struct'
---
```

If I remove `/usr/local/bin` from `$PATH`, it works:
`uv cache clean && PATH=/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin ./0.5.0/uv run example.py`
```
Clearing cache at: /home/gozdal/.cache/uv
Removed 24 files (29.8KiB)
Reading inline script metadata from `example.py`
3.12.3 (main, Nov  6 2024, 18:32:19) [GCC 13.2.0]
```

Similarly if I add `/opt/starfish/python3.12/bin` to `PATH`:
`uv cache clean && PATH=/opt/starfish/python3.12/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin ./0.5.0/uv run example.py`
```
Clearing cache at: /home/gozdal/.cache/uv
Removed 26 files (34.8KiB)
Reading inline script metadata from `example.py`
3.12.5 (main, Nov 14 2024, 04:29:17) [GCC 13.2.0]
```

So it seems that the problem is `/usr/local/bin` symlink.

---

_Comment by @charliermarsh on 2025-01-16 15:15_

Can you please create an MRE in a Dockerfile that I can use to reproduce?

---

_Label `needs-mre` added by @charliermarsh on 2025-01-16 15:15_

---

_Comment by @gozdal on 2025-01-16 17:23_

Sure, have a look at https://github.com/gozdal/sf-python
I reproduced it with:
1. `docker build -t uv-mre .`
2. `docker run -it uv-mre`
3. attach from another console, run `bash uv-0.4.sh` or `bash uv-0.5.sh`

---

_Comment by @gozdal on 2025-01-21 09:27_

@charliermarsh I was wondering if this MRE is good enough for reproduction?

---

_Comment by @charliermarsh on 2025-01-21 14:13_

I think it is, thank you! I just haven't had a chance to fix it yet.

---

_Label `needs-mre` removed by @charliermarsh on 2025-01-21 14:14_

---

_Label `bug` added by @charliermarsh on 2025-01-21 14:14_

---

_Comment by @charliermarsh on 2025-01-21 17:58_

Where does this Python distribution come from? Is it from `python-build-standalone`?

---

_Comment by @gozdal on 2025-01-21 17:59_

It's a custom-built Python, but it's from vanilla, official sources with official build scripts.

---

_Comment by @charliermarsh on 2025-01-21 18:03_

I think this is a problem with your Python distribution. You don't need uv to reproduce it:

```
root@415137aa2e3f:/uv# /usr/local/bin/python3.12 -m venv foo --without-pip
root@415137aa2e3f:/uv# foo/bin/python
Python 3.12.5 (main, Nov 14 2024, 04:29:17) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import struct
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.12/struct.py", line 13, in <module>
    from _struct import *
ModuleNotFoundError: No module named '_struct'
```

It's likely failing to find the expected anchor landmarks during startup. Was it built on a different machine, with a different layout?

---

_Comment by @gozdal on 2025-01-21 18:19_

Hmm, it's built in a container matching the OS.
It doesn't happen on other OS, which do not have Python 3.12 as system package.
My guess is there is some bad interaction between OS Python and our custom one - thought it's something with the venv created by uv but it looks it's deeper.

The Python is built with
```
LDFLAGS="-Wl,--enable-new-dtags,--rpath=/opt/starfish/python3.12/lib" ./configure 
  --prefix=/opt/starfish/python3.12
  --with-computed-gotos
  --disable-test-modules
  --with-ensurepip=no
  --enable-ipv6
  --enable-loadable-sqlite-extensions
  --enable-shared
  --without-static-libpython
  --enable-shared
  --enable-optimizations 
  --with-lto

make -j2
make altinstall
/opt/starfish/python3.12/bin/python3.12 -m ensurepip --upgrade --default-pip
```
and then resulting `/opt/starfish/python3.12` is packaged into DEB.


I understand it's not a `uv` bug but maybe you see something obviously wrong here?

---

_Comment by @zanieb on 2025-01-21 18:26_

The first thing that comes to mind is that it's in `/usr/lib/python3.12` but your prefix is `/opt/starfish/python3.12`?

It's often helpful to look at the paths in the `sysconfig`, e.g.,`python -m sysconfig | grep /usr/lib`

---

_Comment by @charliermarsh on 2025-01-21 18:29_

I would expect that symlink to still be okay given that it's a non-relocatable Python? But I'm not totally sure.

---

_Comment by @gozdal on 2025-01-21 21:59_

It's fine if I run `/opt/starfish/python3.12/bin/python3.12 -m sysconfig | grep /usr/lib`:
```
TZPATH = "/usr/share/zoneinfo:/usr/lib/zoneinfo:/usr/share/lib/zoneinfo:/etc/zoneinfo
```

It's fine if I run via symlink `/usr/local/bin/python3.12 -m sysconfig | grep /usr/lib`:
```
TZPATH = "/usr/share/zoneinfo:/usr/lib/zoneinfo:/usr/share/lib/zoneinfo:/etc/zoneinfo"
```

It's fine if I create venv from the binary: 
`/opt/starfish/python3.12/bin/python3.12 -m venv foo --without-pip`
`foo/bin/python -m sysconfig | grep /usr/lib`
```
TZPATH = "/usr/share/zoneinfo:/usr/lib/zoneinfo:/usr/share/lib/zoneinfo:/etc/zoneinfo"
```

It's not fine if I create the venv using symlink:
`/usr/local/bin/python3.12 -m venv foo --without-pip`
`foo/bin/python -m sysconfig | grep /usr/lib`
```
	stdlib = "/usr/lib/python3.12"
	BINLIBDEST = "/usr/lib/x86_64-linux-gnu/python3.12"
	CONFIG_ARGS = "'--enable-shared' '--prefix=/usr' '--libdir=/usr/lib/x86_64-linux-gnu' '--enable-ipv6' '--enable-loadable-sqlite-extensions' '--with-dbmliborder=bdb:gdbm' '--with-computed-gotos' '--without-ensurepip' '--with-system-expat' '--with-dtrace' '--with-ssl-default-suites=openssl' '--with-wheel-pkg-dir=/usr/share/python-wheels/' 'MKDIR_P=/bin/mkdir -p' 'CC=x86_64-linux-gnu-gcc'"
	DESTDIRS = "/usr /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/python3.12 /usr/lib/python3.12/lib-dynload"
	DESTLIB = "/usr/lib/python3.12"
	DESTSHARED = "/usr/lib/python3.12/lib-dynload"
	LIBDEST = "/usr/lib/python3.12"
	LIBDIR = "/usr/lib/x86_64-linux-gnu"
	LIBPC = "/usr/lib/x86_64-linux-gnu/pkgconfig"
	LIBPL = "/usr/lib/python3.12/config-3.12-x86_64-linux-gnu"
	MACHDESTLIB = "/usr/lib/x86_64-linux-gnu/python3.12"
	MODULE__HASHLIB_LDFLAGS = "-L/usr/lib   -lcrypto"
	MODULE__SSL_LDFLAGS = "-L/usr/lib  -lssl -lcrypto"
	SCRIPTDIR = "/usr/lib"
	TZPATH = "/usr/share/zoneinfo:/usr/lib/zoneinfo:/usr/share/lib/zoneinfo:/etc/zoneinfo"
	WASM_STDLIB = "./usr/lib/python3.12/os.py"
	srcdir = "/usr/lib/python3.12/config-3.12-x86_64-linux-gnu"
```

But I have no idea why this happens, because `foo/bin/python` is "just" a symlink to symlink:
`ls -al foo/bin/`
```
total 32
drwxr-xr-x 2 gozdal core-devs 4096 Jan 21 16:56 .
drwxr-xr-x 5 gozdal core-devs 4096 Jan 21 16:56 ..
-rw-r--r-- 1 gozdal core-devs 2018 Jan 21 16:56 activate
-rw-r--r-- 1 gozdal core-devs  908 Jan 21 16:56 activate.csh
-rw-r--r-- 1 gozdal core-devs 2187 Jan 21 16:56 activate.fish
-rw-r--r-- 1 gozdal core-devs 9033 Jan 21 16:56 Activate.ps1
lrwxrwxrwx 1 gozdal core-devs   10 Jan 21 16:56 python -> python3.12
lrwxrwxrwx 1 gozdal core-devs   10 Jan 21 16:56 python3 -> python3.12
lrwxrwxrwx 1 gozdal core-devs   25 Jan 21 16:56 python3.12 -> /usr/local/bin/python3.12
```

---

_Comment by @gozdal on 2025-01-21 22:01_

Merely creating a symlink to a symlink doesn't trigger this.
`ln -s /usr/local/bin/python3.12 usrlocalbinpython3.12`
`./usrlocalbinpython3.12 -m sysconfig | grep /usr/lib`
```
	TZPATH = "/usr/share/zoneinfo:/usr/lib/zoneinfo:/usr/share/lib/zoneinfo:/etc/zoneinfo"
```

---

_Comment by @zanieb on 2025-01-21 22:10_

I wouldn't expect anything in `sysconfig` to change from a symlink. That was just a suggestion of how to inspect what kind of expectations are encoded into the build.

---

_Comment by @zanieb on 2025-01-21 22:11_

See

- https://github.com/python/cpython/issues/106045

---

_Comment by @gozdal on 2025-01-21 22:18_

@zanieb that sounds exactly like my problem, thanks!
I wonder how `python-build-standalone` works around this problem.

---

_Comment by @zanieb on 2025-01-21 22:23_

We implement custom behavior to work around that https://github.com/astral-sh/uv/blob/cecff3a72630a23287b32efa8fbec20d31a85b05/crates/uv-virtualenv/src/virtualenv.rs#L65-L86

---

_Comment by @zanieb on 2025-01-21 22:24_

We may upstream it somehow, but it's harder to make changes like this upstream than here. You may want to weigh in on that issue.

---

_Label `bug` removed by @charliermarsh on 2025-01-22 00:06_

---

_Label `upstream` added by @charliermarsh on 2025-01-22 00:06_

---

_Comment by @gozdal on 2025-01-22 16:49_

Thanks a lot @zanieb @charliermarsh , that was very helpful.

---

_Closed by @gozdal on 2025-01-22 16:49_

---

_Referenced in [astral-sh/uv#11234](../../astral-sh/uv/issues/11234.md) on 2025-02-05 06:45_

---

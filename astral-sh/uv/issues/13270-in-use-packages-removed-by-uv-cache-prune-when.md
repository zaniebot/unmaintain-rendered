```yaml
number: 13270
title: "In-use Packages Removed by `uv cache prune` When Using Symlinks"
type: issue
state: closed
author: xyu
labels:
  - needs-mre
assignees: []
created_at: 2025-05-02T17:18:12Z
updated_at: 2025-05-10T18:39:12Z
url: https://github.com/astral-sh/uv/issues/13270
synced_at: 2026-01-12T16:01:23Z
```

# In-use Packages Removed by `uv cache prune` When Using Symlinks

---

_@xyu_

### Summary

It appears the hash rather than package name change in #11738 introduced a bug in `find_archive_references()` and cache pruning now removes some libraries that are still in use. I created the following test file to demonstrate the issue:

```bash
#!/bin/bash
set -eu

VER="$1"

rm -rf "$VER"
uv venv "$VER"
cd "$VER"
source bin/activate

set -x

uv pip install "uv==$VER"
bin/uv --version

UV_LINK_MODE=symlink bin/uv pip install zstandard
symlinks lib/python3.*/site-packages/zstandard

bin/uv cache -v prune
symlinks lib/python3.*/site-packages/zstandard
```

When running this with `./uv-test.sh 0.6.4` I get the following output:

```
Using CPython 3.11.12 interpreter at: /opt/Python/bin/python3
Creating virtual environment at: 0.6.4
+ uv pip install uv==0.6.4
Using Python 3.11.12 environment at:
Resolved 1 package in 1.00s
Prepared 1 package in 8.45s
Installed 1 package in 2ms
 + uv==0.6.4
+ bin/uv --version
uv 0.6.4
+ bin/uv pip install zstandard
Using Python 3.11.12 environment at:
Resolved 1 package in 604ms
Prepared 1 package in 2.25s
Installed 1 package in 3ms
 + zstandard==0.23.0
+ symlinks lib/python3.11/site-packages/zstandard
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/__init__.py -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/__init__.py
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/backend_cffi.py -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/backend_cffi.py
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/py.typed -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/py.typed
other_fs: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/__init__.pyi -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/__init__.pyi
+ bin/uv cache -v prune
DEBUG uv 0.6.4
Pruning cache at: /home/remote-user/.cache/uv
DEBUG Removing dangling cache bucket: /home/remote-user/.cache/uv/wheels-v4
DEBUG Removing dangling cache archive: /app/data/wyeast/caches/.cache/uv/archive-v0/VIcOQ_j9JoOfTDcJ0PkGp
DEBUG Removing dangling cache archive: /app/data/wyeast/caches/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs
Removed 26 files (52.5MiB)
+ symlinks lib/python3.11/site-packages/zstandard
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/__init__.py -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/__init__.py
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/backend_cffi.py -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/backend_cffi.py
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/py.typed -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/py.typed
dangling: /home/remote-user/0.6.4/lib/python3.11/site-packages/zstandard/__init__.pyi -> /home/remote-user/.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs/zstandard/__init__.pyi
```

So after installing `zstandard` then calling `uv cache prune` the archive dir `.cache/uv/archive-v0/3XkaxKI_hOb98TWKNRvUs` where that package gets installed gets removed thus leaving dangling symlinks in the package venv.

Compared to the previous version, `0.6.3` where the archive dir remains untouched after a cache prune:

```
Using CPython 3.11.12 interpreter at: /opt/Python/bin/python3
Creating virtual environment at: 0.6.3
+ uv pip install uv==0.6.3
Using Python 3.11.12 environment at:
Resolved 1 package in 1.46s
Prepared 1 package in 9.16s
Installed 1 package in 3ms
 + uv==0.6.3
+ bin/uv --version
uv 0.6.3
+ bin/uv pip install zstandard
Using Python 3.11.12 environment at:
Resolved 1 package in 982ms
Prepared 1 package in 2.88s
Installed 1 package in 2ms
 + zstandard==0.23.0
+ symlinks lib/python3.11/site-packages/zstandard
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/__init__.py -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/__init__.py
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/backend_cffi.py -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/backend_cffi.py
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/py.typed -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/py.typed
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/__init__.pyi -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/__init__.pyi
+ bin/uv cache -v prune
DEBUG uv 0.6.3
Pruning cache at: /home/remote-user/.cache/uv
DEBUG Removing dangling cache bucket: /home/remote-user/.cache/uv/wheels-v5
Removed 3 files (1.4KiB)
+ symlinks lib/python3.11/site-packages/zstandard
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/backend_c.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/__init__.py -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/__init__.py
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/_cffi.cpython-311-aarch64-linux-gnu.so
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/backend_cffi.py -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/backend_cffi.py
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/py.typed -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/py.typed
other_fs: /home/remote-user/0.6.3/lib/python3.11/site-packages/zstandard/__init__.pyi -> /home/remote-user/.cache/uv/archive-v0/c8tiDgnJBm77dsv16VIVJ/zstandard/__init__.pyi
```

### Platform

Debian 12, aarch64 and x86_64

### Version

0.6.4+

### Python version

3.11.12, 3.12.10, 3.13.3

---

_Label `bug` added by @xyu on 2025-05-02 17:18_

---

_Comment by @charliermarsh on 2025-05-04 14:41_

Unfortunately I haven't been able to reproduce this. When I run that script, none of the archives are removed. Could your cache be in a weird state? Do you see the same thing after running `uv cache clean`?

---

_Label `bug` removed by @charliermarsh on 2025-05-04 17:50_

---

_Label `needs-mre` added by @charliermarsh on 2025-05-04 17:50_

---

_Comment by @xyu on 2025-05-05 17:12_

> Unfortunately I haven't been able to reproduce this. When I run that script, none of the archives are removed.

Ok I took another look and it looks like it's because I'm using symlinks instead of hardlinks. I amended the test script to add `UV_LINK_MODE=symlink` can you try again @charliermarsh? I have to use symlinks because we are running this in Dockerized environments and we want mount the cache dir into the container so that we can persist it across invocations for speed.

---

_Renamed from "In-use Packages Removed by `uv cache prune`" to "In-use Packages Removed by `uv cache prune` When Using Symlinks" by @xyu on 2025-05-05 17:15_

---

_Comment by @charliermarsh on 2025-05-07 22:35_

Unfortunately I still see roughly the same thing:

```
+ symlinks lib/python3.13/site-packages/zstandard
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/_cffi.cpython-313-darwin.so -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/_cffi.cpython-313-darwin.so
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/__init__.pyi -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/__init__.pyi
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/__init__.py -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/__init__.py
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/backend_cffi.py -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/backend_cffi.py
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/py.typed -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/py.typed
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/backend_c.cpython-313-darwin.so -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/backend_c.cpython-313-darwin.so
+ bin/uv cache -v prune
DEBUG uv 0.7.0 (62bca8c34 2025-04-29)
Pruning cache at: /Users/crmarsh/.cache/uv
No unused entries found
+ symlinks lib/python3.13/site-packages/zstandard
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/_cffi.cpython-313-darwin.so -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/_cffi.cpython-313-darwin.so
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/__init__.pyi -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/__init__.pyi
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/__init__.py -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/__init__.py
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/backend_cffi.py -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/backend_cffi.py
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/py.typed -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/py.typed
absolute: /Users/crmarsh/workspace/puffin/foo/0.7.0/lib/python3.13/site-packages/zstandard/backend_c.cpython-313-darwin.so -> /Users/crmarsh/.cache/uv/archive-v0/rVEu_epLYxeo6MSInGqIh/zstandard/backend_c.cpython-313-darwin.so
```


---

_Comment by @charliermarsh on 2025-05-07 22:36_

Are you able to reproduce this in 0.7.0? Could it be an effect of switching uv versions, or do you still see this if you run `uv cache clean` first?

---

_Comment by @xyu on 2025-05-08 18:27_

> Are you able to reproduce this in 0.7.0? Could it be an effect of switching uv versions, or do you still see this if you run uv cache clean first?

I'm able to reproduce this in 0.7.0 and it does not matter if I run `uv cache clean` or not.

It looks like this issue is not reproducible on MacOS / Darwin but it is reproducible on Debian / Linux. To clarify the issue further I've pushed a Docker image, `hypertextranch/uv-13270`, to demonstrate the issue:

```
$ docker run --rm hypertextranch/uv-13270:latest 0.7.0

Testing issue #13270 for uv 0.7.0 on Debian GNU/Linux 12 (bookworm)

...snip...
DEBUG Removing dangling cache archive: /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK
Removed 11 files (20.6MiB)
+ symlinks lib/python3.13/site-packages/zstandard
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/__init__.py -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/__init__.py
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/backend_c.cpython-313-aarch64-linux-gnu.so -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/backend_c.cpython-313-aarch64-linux-gnu.so
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/backend_cffi.py -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/backend_cffi.py
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/py.typed -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/py.typed
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/__init__.pyi -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/__init__.pyi
dangling: /0.7.0/lib/python3.13/site-packages/zstandard/_cffi.cpython-313-aarch64-linux-gnu.so -> /root/.cache/uv/archive-v0/IVFKnhpbv3zp64XpoMjIK/zstandard/_cffi.cpython-313-aarch64-linux-gnu.so
```


```
$ docker run --rm hypertextranch/uv-13270:latest 0.6.3

Testing issue #13270 for uv 0.6.3 on Debian GNU/Linux 12 (bookworm)

...snip...
No unused entries found
+ symlinks lib/python3.13/site-packages/zstandard
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/__init__.py -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/__init__.py
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/backend_c.cpython-313-aarch64-linux-gnu.so -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/backend_c.cpython-313-aarch64-linux-gnu.so
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/backend_cffi.py -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/backend_cffi.py
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/py.typed -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/py.typed
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/__init__.pyi -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/__init__.pyi
absolute: /0.6.3/lib/python3.13/site-packages/zstandard/_cffi.cpython-313-aarch64-linux-gnu.so -> /root/.cache/uv/archive-v0/odEgNMndS5fioMfLw05D7/zstandard/_cffi.cpython-313-aarch64-linux-gnu.so
```

The `Dockerfile` and `uv-test.sh` used to build this image is here:
https://gist.github.com/xyu/a546a6e37c40069cede1a1973b8f806e

Note: I've only built this image for arm64 assuming you are using an M* series machine, please let me know if you are testing under x86_64, I can build a multiarch image for this as well.

---

_Comment by @charliermarsh on 2025-05-08 19:38_

Oh great, thanks! Yes, I've only been testing on macOS. I'll take another look.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-08 19:38_

---

_Comment by @charliermarsh on 2025-05-09 04:08_

Confirmed I also see it when running that Docker image.

---

_Comment by @charliermarsh on 2025-05-10 18:15_

Thanks, I've found the bug (we're messing up when the wheel tag contains a dot).

---

_Closed by @charliermarsh on 2025-05-10 18:39_

---

_Closed by @charliermarsh on 2025-05-10 18:39_

---

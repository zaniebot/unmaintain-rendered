---
number: 3578
title: "`uv venv` does not create a lib64 directory"
type: issue
state: closed
author: AtomBaf
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-05-14T14:08:55Z
updated_at: 2024-05-14T18:33:45Z
url: https://github.com/astral-sh/uv/issues/3578
synced_at: 2026-01-10T01:23:29Z
---

# `uv venv` does not create a lib64 directory

---

_Issue opened by @AtomBaf on 2024-05-14 14:08_

# Context
Platform: linux rocky 9
uv version: `0.1.42`

# Error when importing torch
```bash
uv venv foobar
source foobar/bin/activate
uv pip install torch
python -c "import torch"
> ImportError: libcudnn.so.8: cannot open shared object file: No such file or directory
```
# Investigation
After investigation I discovered that:
 - `uv venv foobar` does not create a `lib64` directory under the venv directory
 - installing pytorch will create a lib64 directory with some libraries (`nvidia*`)  inside it
 - in fact there will be some libraries in `lib` and some other in `lib64`
 - this `lib64` is part of the `sys.path` , however, it seems not used during the import of torch thus leading to the error above
 - with a similar `python -m venv foobar` , I noticed that the `lib64` directory is created as a symlink to the `lib` directory
 - indeed, after `uv venv`, when manually symlinking, everything is fine


---

_Comment by @charliermarsh on 2024-05-14 14:15_

Does `virtualenv` do this? I suspect the distro is patching the `venv` module. I'm not sure how we could know the create the symlink here.

---

_Comment by @henryiii on 2024-05-14 16:09_

This is https://github.com/python/cpython/blob/b228655c227b2ca298a8ffac44d14ce3d22f6faa/Lib/venv/__init__.py#L135-L140 (basically, it's always created on 64-bit non-OS X POSIX)

---

_Comment by @charliermarsh on 2024-05-14 16:40_

Lol thank you. Will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-14 16:40_

---

_Label `bug` added by @charliermarsh on 2024-05-14 16:40_

---

_Label `compatibility` added by @charliermarsh on 2024-05-14 16:40_

---

_Referenced in [astral-sh/uv#3584](../../astral-sh/uv/pulls/3584.md) on 2024-05-14 17:47_

---

_Comment by @konstin on 2024-05-14 18:03_

`virtualenv .venv --no-seed` (or with seed) doesn't create `lib64` for me (ubuntu 24.04 x86_64), `python -m venv` does:

```
.venv
├── bin
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── activate.nu
│   ├── activate.ps1
│   ├── activate_this.py
│   ├── python -> /home/konsti/.pyenv/versions/3.12.3/bin/python3.12
│   ├── python3 -> python
│   └── python3.12 -> python
├── lib
│   └── python3.12
│       └── site-packages
│           ├── _virtualenv.pth
│           └── _virtualenv.py
└── pyvenv.cfg

5 directories, 12 files
```

---

_Closed by @charliermarsh on 2024-05-14 18:33_

---

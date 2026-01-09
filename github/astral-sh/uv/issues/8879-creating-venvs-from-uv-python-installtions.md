---
number: 8879
title: Creating venvs from uv python installtions creates broken venvs
type: issue
state: open
author: Olindholm
labels:
  - external
assignees: []
created_at: 2024-11-07T06:30:45Z
updated_at: 2025-06-10T20:55:41Z
url: https://github.com/astral-sh/uv/issues/8879
synced_at: 2026-01-07T13:12:18-06:00
---

# Creating venvs from uv python installtions creates broken venvs

---

_Issue opened by @Olindholm on 2024-11-07 06:30_

I am using uv to control my python version, great stuff. I create a venv using uv `uv venv --python ...` and uv will install whatever version I requrest. Works great.

I am also using an internal python package which creates it's own python venv in order to run some commands in isolation. Using regular python this works fine, but when using uv's managed installed pythons it does not work.

I believe I have identified the problem being that the package creates venvs using copy, rather than symlink. The issue can be replicated the following way.
```sh
# Create a venv with uv managed version
uv venv --python 3.8
source .venv/bin/activate

# Create a venv using copy
python -m venv .venv2 --copies # This command will yield and error:
Error: Command '['/home/username/.venv2/bin/python', '-Im', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 127.

# To understand why this failed, let's run that command ourselves
$ /home/username/.venv2/bin/python -Im ensurepip --upgrade --default-pip # This will also error:
/home/username/.venv2/bin/python: error while loading shared libraries: /home/username/.venv2/bin/../lib/libpython3.8.so.1.0: cannot open shared object file: No such file or directory
```
Note: Creating your venv using `--symlinks` flag instead, works great.

As I understand it, when creating venvs using copy, `python -m venv --copies` or `import venv; venv.create(symlinks=False)`, the python executable is copied, as you'd expect. But uv's python binary is different from regular one, which has links to files which don't get copied. Yielding the venv broken.

---

_Comment by @konstin on 2024-11-07 09:16_

This sounds like another case of https://github.com/indygreg/python-build-standalone/issues/380

---

_Label `upstream` added by @charliermarsh on 2024-11-07 13:33_

---

_Comment by @charliermarsh on 2024-11-07 13:33_

`uv venv` itself doesn't support `--copies` so in some sense this isn't a uv issue but a python-build-standalone issue.

---

_Comment by @zanieb on 2024-11-07 13:51_

Similar to https://github.com/astral-sh/uv/issues/8821

Note there's not a clear path to solving this upstream unless we want to patch CPython runtime behavior. I think installers of the distributions may need to patch the sysconfig to fix this as described in #8429 



---

_Comment by @charliermarsh on 2024-11-07 13:52_

Yeah I have no idea how this would work for `--copies` since we lose all connection to the originating distribution.

---

_Referenced in [astral-sh/uv#8953](../../astral-sh/uv/issues/8953.md) on 2024-11-09 19:24_

---

_Referenced in [astral-sh/uv#12173](../../astral-sh/uv/issues/12173.md) on 2025-03-14 19:05_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-10 20:55_

---

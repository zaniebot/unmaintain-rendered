```yaml
number: 10654
title: "`uv.lock` generated with `uv==0.5.18` fails to parse in `uv==0.5.19`"
type: issue
state: closed
author: chebbyChefNEQ
labels:
  - bug
assignees: []
created_at: 2025-01-15T21:48:07Z
updated_at: 2025-01-15T23:03:30Z
url: https://github.com/astral-sh/uv/issues/10654
synced_at: 2026-01-10T04:27:58Z
```

# `uv.lock` generated with `uv==0.5.18` fails to parse in `uv==0.5.19`

---

_Issue opened by @chebbyChefNEQ on 2025-01-15 21:48_

error:
```
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 2823, column 10
     |
2823 | wheels = [
     |          ^
failed to parse `watchdog-6.0.0-py3-none-win_ia64.whl` as wheel filename: The wheel filename "watchdog-6.0.0-py3-none-win_ia64.whl" has an invalid platform tag: Unknown platform tag format: win_ia64
```

repro:
```toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["watchdog==6.0.0"]
```

command `rm uv.lock && pip install uv==0.5.18 && uv lock && pip install uv==0.5.19 && uv lock`

running `rm uv.lock && pip install uv==0.5.19 && uv lock` is successful <=== **This is the workaround** (delete and regen the lockfile)

---

_Comment by @charliermarsh on 2025-01-15 21:48_

Makes sense, thanks. 

---

_Label `bug` added by @charliermarsh on 2025-01-15 21:48_

---

_Comment by @Drewroen on 2025-01-15 21:51_

We are running into the same issue with uv 0.5.19 with both watchdog and pycryptodome as another data point

Command: `uv sync --frozen --no-dev`

```
0.178 Using CPython 3.11.11 interpreter at: /usr/local/bin/python3.11
0.178 Creating virtual environment at: .venv
0.183 error: Failed to parse `uv.lock`
0.183   Caused by: TOML parse error at line 1403, column 10
0.183      |
0.183 1403 | wheels = [
0.183      |          ^
0.183 failed to parse `pycryptodome-3.18.0-pp27-pypy_73-manylinux2010_x86_64.whl` as wheel filename: The wheel filename "pycryptodome-3.18.0-pp27-pypy_73-manylinux2010_x86_64.whl" has an invalid ABI tag: Missing major version in PyPy ABI tag: pypy_73
```

---

_Comment by @donovan-sevenai on 2025-01-15 21:55_

Looks like this was already somewhat identified: https://github.com/astral-sh/uv/blob/main/crates/uv-resolver/src/lock/mod.rs#L308

> // TODO(charlie): This omits `win_ia64`, which is accepted by Warehouse.

---

_Comment by @charliermarsh on 2025-01-15 21:56_

Omitting the tag is fine — we never supported it, so uv never would’ve selected it. The problem here is just that we can’t deserialize it from an existing lockfile.

---

_Comment by @zanieb on 2025-01-15 21:58_

If you delete the entry from the lockfile, it should solve the problem without any other effect (since, as Charlie said, we don't support selecting those wheels anyway).

We'll see if there's a reasonable way to support deserializing unknown tags from existing lockfiles.

---

_Comment by @charliermarsh on 2025-01-15 22:04_

I'll try to fix this now, sorry.

---

_Comment by @mgcrea on 2025-01-15 22:30_

Hitting this as well, broke on CI (using a python:3.12 image) as it pulled the latest uv version:

```
$ pip install uv
Collecting uv
  Downloading uv-0.5.19-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
Downloading uv-0.5.19-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16.0/16.0 MB 135.2 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.5.19
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
$ uv sync --frozen
Using CPython 3.12.8 interpreter at: /usr/local/bin/python3
Creating virtual environment at: .venv
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 1085, column 10
     |
1085 | wheels = [
     |          ^
failed to parse `watchdog-6.0.0-py3-none-win_ia64.whl` as wheel filename: The wheel filename "watchdog-6.0.0-py3-none-win_ia64.whl" has an invalid platform tag: Unknown platform tag format: win_ia64
```

---

_Comment by @charliermarsh on 2025-01-15 22:33_

Can you please include the error message for my own records?

---

_Comment by @mgcrea on 2025-01-15 22:35_

> Can you please include the error message for my own records?

done :) 

---

_Comment by @charliermarsh on 2025-01-15 22:41_

https://github.com/astral-sh/uv/pull/10655 is up which fixes this; we'll have a release out once it's merged.

---

_Comment by @sunshowers on 2025-01-15 22:48_

Thanks for the easy workaround btw! Removing the win_ia64 wheel was quite easy.

---

_Comment by @charliermarsh on 2025-01-15 22:50_

Thanks for confirming @sunshowers and sorry for the disruption.

---

_Closed by @charliermarsh on 2025-01-15 23:03_

---

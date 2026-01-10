```yaml
number: 17060
title: UV took the wrong torch nightly for the Linux and Windows Platform
type: issue
state: open
author: lanluo-nvidia
labels:
  - bug
assignees: []
created_at: 2025-12-10T00:55:18Z
updated_at: 2025-12-12T23:26:50Z
url: https://github.com/astral-sh/uv/issues/17060
synced_at: 2026-01-10T03:11:35Z
```

# UV took the wrong torch nightly for the Linux and Windows Platform

---

_Issue opened by @lanluo-nvidia on 2025-12-10 00:55_

### Summary

I ran uv lock to update the uv.lock file with torch nightly

it updateD the uv.lock with version: 
windows: 2.10.0.dev20251208+cu130  
linux:        2.10.0.dev20251209+cu130 (this version does not even exist in pytorch nightly index)
https://download.pytorch.org/whl/nightly/torch/ 
at the time I ran this uv lock, the latest version is as below
windows: 2.10.0.dev20251209+cu130 
linux:        2.10.0.dev20251208+cu130
the uv.lock file seems like mismatch linux with windows version, and mismatched windows with linux version


### Platform

Linux 6.14.0-37-generic x86_64 GNU/Linux

### Version

astral-sh/setup-uv@v7

### Python version

3.11

---

_Label `bug` added by @lanluo-nvidia on 2025-12-10 00:55_

---

_Comment by @charliermarsh on 2025-12-10 00:56_

Can you please include a complete reproduction? What was the state of your `uv.lock` file and `pyproject.toml` when you ran `uv.lock`?

---

_Comment by @lanluo-nvidia on 2025-12-10 00:57_

Here is the wrong torch record in the uv.lock file:
[[package]]
name = "torch"
version = "2.10.0.dev20251208+cu130"
source = { registry = "https://download.pytorch.org/whl/nightly/cu130" }
resolution-markers = [
    "python_full_version >= '3.13' and sys_platform == 'windows'",
    "python_full_version == '3.12.*' and sys_platform == 'windows'",
    "python_full_version < '3.12' and sys_platform == 'windows'",
]
dependencies = [
    { name = "filelock", marker = "sys_platform == 'windows'" },
    { name = "fsspec", marker = "sys_platform == 'windows'" },
    { name = "jinja2", marker = "sys_platform == 'windows'" },
    { name = "networkx", marker = "sys_platform == 'windows'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and sys_platform == 'windows'" },
    { name = "sympy", marker = "sys_platform == 'windows'" },
    { name = "typing-extensions", marker = "sys_platform == 'windows'" },
]

[[package]]
name = "torch"
version = "2.10.0.dev20251209+cu130"
source = { registry = "https://download.pytorch.org/whl/nightly/cu130" }
resolution-markers = [
    "python_full_version >= '3.13' and sys_platform == 'linux'",
    "python_full_version == '3.12.*' and sys_platform == 'linux'",
    "python_full_version < '3.12' and sys_platform == 'linux'",
]
dependencies = [
    { name = "filelock", marker = "sys_platform == 'linux'" },
    { name = "fsspec", marker = "sys_platform == 'linux'" },
    { name = "jinja2", marker = "sys_platform == 'linux'" },
    { name = "networkx", marker = "sys_platform == 'linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and sys_platform == 'linux'" },
    { name = "sympy", marker = "sys_platform == 'linux'" },
    { name = "typing-extensions", marker = "sys_platform == 'linux'" },
]


---

_Comment by @lanluo-nvidia on 2025-12-10 00:59_

Here is the full uv lock verbose log:

[uv.log](https://github.com/user-attachments/files/24066234/uv.log)

---

_Comment by @charliermarsh on 2025-12-10 01:02_

I don't fully understand what the problem is here, but note that `sys_platform == 'windows'` is incorrect -- you're looking for `sys_platform == 'win32'`.

---

_Comment by @lanluo-nvidia on 2025-12-10 01:03_

hi @charliermarsh  thanks for your swift response. 
repro steps are as below:
Our repo: https://github.com/pytorch/TensorRT
switch to  branch: lluo/add_uv_sync_job
install bazel:
uv lock
uv sync --frozen

https://github.com/pytorch/TensorRT/blob/8bc31c83bb5ea8bcdb4737134ad1ceaf8826a6a0/.github/workflows/uv-update.yml



---

_Comment by @charliermarsh on 2025-12-10 01:04_

Thank you, I will take a look.

---

_Comment by @lanluo-nvidia on 2025-12-10 01:07_

I have changed from 
sys_platform == 'windows'
to 
sys_platform == 'win32'
same wrong version

---

_Comment by @charliermarsh on 2025-12-10 03:13_

Unfortunately I can't build that project as I'm on macOS.

---

_Comment by @lanluo-nvidia on 2025-12-11 22:27_

Hi @charliermarsh 
Since you cannot repro on your mac, can you please look at the result I am getting from the uv lock or uv sync:
Is it a bug on either
Reads/sorts platform-specific nightly wheels from that index, 
or
Emits resolution-markers in the lock for multi-platform packages.

uv.lock file generate the torch nightly records for linux :
2.10.0.dev20251211+cu130
but this version does not even exist in https://download.pytorch.org/whl/nightly/cu130
(you can see there is only windows wheel for torch on this day)
```
[[package]]
name = "torch"
version = "2.10.0.dev20251211+cu130"
source = { registry = "https://download.pytorch.org/whl/nightly/cu130" }
resolution-markers = [
    "python_full_version >= '3.13' and sys_platform == 'linux'",
    "python_full_version == '3.12.*' and sys_platform == 'linux'",
    "python_full_version == '3.11.*' and sys_platform == 'linux'",
    "python_full_version < '3.11' and sys_platform == 'linux'",
]
dependencies = [
    { name = "filelock", marker = "sys_platform == 'linux'" },
    { name = "fsspec", marker = "sys_platform == 'linux'" },
    { name = "jinja2", marker = "sys_platform == 'linux'" },
    { name = "networkx", version = "3.4.2", source = { registry = "https://download.pytorch.org/whl/nightly/cu130" }, marker = "python_full_version < '3.11' and sys_platform == 'linux'" },
    { name = "networkx", version = "3.6.1", source = { registry = "https://download.pytorch.org/whl/nightly/cu130" }, marker = "python_full_version >= '3.11' and sys_platform == 'linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and sys_platform == 'linux'" },
    { name = "sympy", marker = "sys_platform == 'linux'" },
    { name = "typing-extensions", marker = "sys_platform == 'linux'" },
]
```

---

_Comment by @lanluo-nvidia on 2025-12-11 22:28_

it is become a major issue, since nightly windows, linux wheel are not always there for all the platforms.
This is keeping giving me the wrong non-exist index for torch.

---

_Comment by @lanluo-nvidia on 2025-12-11 22:30_

it is working only if it has the torch wheels for all the platform on that nightly.


---

_Comment by @lanluo-nvidia on 2025-12-11 23:28_

I am able to make it work by:
1) adding required-environments: 
environments = ["sys_platform == 'linux'", "sys_platform == 'win32'"]
```
required-environments = [
    "sys_platform == 'linux' and python_version >= '3.10' and python_version <= '3.13' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and python_version >= '3.10' and python_version <= '3.13' and platform_machine == 'AMD64'"
]
```
2) use `uv lock --refresh --prerelease=allow` instead of `uv lock`

@charliermarsh  I Would like to hear from you is this the correct way to do it?


---

_Comment by @konstin on 2025-12-12 23:26_

Using a windows-only version for a linux environments is indeed bad, it looks like the same problem as https://github.com/astral-sh/uv/issues/17067, which has a minimal example.

I couldn't verify with the actual repo cause my build got stuck at "Loading: 0 packages loaded", but adding this should work:

```toml
# We must have wheel for both of those platforms
required-environments = ["sys_platform == 'linux'", "sys_platform == 'windows'"]
# Ignore other platforms
environments = ["sys_platform == 'linux'", "sys_platform == 'windows'"]
```

---

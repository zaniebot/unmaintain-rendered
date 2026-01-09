---
number: 7370
title: "Can't install nvidia-cublas from a wheel despite a good looking lock"
type: issue
state: closed
author: wokalski
labels:
  - question
assignees: []
created_at: 2024-09-13T18:02:08Z
updated_at: 2024-09-13T19:55:24Z
url: https://github.com/astral-sh/uv/issues/7370
synced_at: 2026-01-07T13:12:17-06:00
---

# Can't install nvidia-cublas from a wheel despite a good looking lock

---

_Issue opened by @wokalski on 2024-09-13 18:02_

I am trying to install torch using uv. I am developing on the mac and running `uv sync`[1] during docker build.

i am getting:


```
=> ERROR [5/9] RUN uv sync -v --frozen                                                              1.7s
------                                                                                                    
 > [5/9] RUN uv sync -v --frozen:                                                                         
#0 0.232 <jemalloc>: MADV_DONTNEED does not work (memset will be used instead)                            
#0 0.232 <jemalloc>: (This is the expected behaviour if you are running under QEMU)                       
#0 0.380 DEBUG uv 0.4.9                                                                                   
#0 0.384 DEBUG Found project root: `/app`                                                                 
#0 0.385 DEBUG No workspace root found, using project root
#0 0.390 DEBUG Searching for Python ==3.10.* in managed installations or system path
#0 0.390 DEBUG Searching for managed installations at `/root/.local/share/uv/python`
#0 1.652 DEBUG Found `cpython-3.10.15-linux-x86_64-musl` at `/usr/local/bin/python3` (search path)
#0 1.653 Using Python 3.10.15 interpreter at: /usr/local/bin/python3
#0 1.655 Creating virtualenv at: .venv
#0 1.693 error: distribution nvidia-cublas-cu11==11.11.3.6 @ registry+https://download.pytorch.org/whl/cu118 can't be installed because it doesn't have a source distribution or wheel for the current platform
------
```

Here's my pyproject.toml:

```
[project]
name = "tts"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = [
    "torch==2.4.0+cu118; sys_platform == 'linux' and platform_machine == 'x86_64'",
]

[tool.uv]
environments = [
  "sys_platform == 'linux'",
]
dev-dependencies = [
]
find-links = [
]
extra-index-url = [
    "https://download.pytorch.org/whl/cu118",
]

[tool.uv.sources]

[tool.pytest.ini_options]
```

and here's a relevant snippet from the lock:

```
[[package]]
name = "nvidia-cublas-cu11"
version = "11.11.3.6"
source = { registry = "https://download.pytorch.org/whl/cu118" }
wheels = [
    { url = "https://download.pytorch.org/whl/cu118/nvidia_cublas_cu11-11.11.3.6-py3-none-manylinux1_x86_64.whl", hash = "sha256:39fb40e8f486dd8a2ddb8fdeefe1d5b28f5b99df01c87ab3676f057a74a5a6f3" },
]
```

which looks good.

and if you'd like to repro, here's a simple docker file:

```
FROM ghcr.io/astral-sh/uv:python3.10-alpine

WORKDIR /app

COPY pyproject.toml uv.lock .

RUN uv sync -v --frozen
```

I don't understand the error at all and at this point I'm dumbfounded and i don't know what to do.

[1]: Running `uv sync` without a lockfile yields the same error


---

_Renamed from "can't install nvidia-cublas from a wheel despite a good looking lock" to "Can't install nvidia-cublas from a wheel despite a good looking lock" by @wokalski on 2024-09-13 18:02_

---

_Comment by @charliermarsh on 2024-09-13 18:14_

Are you in an Alpine container? It looks like you have a musl Python, but the wheel in the lockfile is for `manylinux`. Can you try with a non-musl Docker container?

---

_Label `question` added by @charliermarsh on 2024-09-13 18:14_

---

_Comment by @wokalski on 2024-09-13 19:52_

You are 100% right. Rookie mistake.

---

_Closed by @wokalski on 2024-09-13 19:52_

---

_Comment by @charliermarsh on 2024-09-13 19:55_

Not at all, I had no idea what musl was not long ago!

---

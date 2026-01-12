```yaml
number: 13329
title: Stack overflow
type: issue
state: closed
author: JWES1971
labels:
  - needs-mre
assignees: []
created_at: 2025-05-07T12:48:32Z
updated_at: 2025-07-11T02:15:38Z
url: https://github.com/astral-sh/uv/issues/13329
synced_at: 2026-01-12T16:01:25Z
```

# Stack overflow

---

_@JWES1971_

### Summary

Using uv run main.py in Ubuntu 24.04 is returning the following error:

Using CPython 3.13.3
Creating virtual environment at: .venv
⠙ Resolving dependencies...                                                                                                 
thread 'uv-resolver' has overflowed its stack
fatal runtime error: stack overflow
Abortado (imagem do núcleo gravada)

I did an uninstall, and after that, I installed UV once more, but the error still occurs.

I tested on another machine with Ubuntu 22.04 with no error.

Any clue about it?

Information about the system used:

OS: Ubuntu trixie/sid (noble) x86_64 
Host: X570 AORUS ELITE -CF 
Kernel: 6.11.0-25-generic 
Shell: bash 5.2.21 
DE: GNOME 46.0 (x11) 
Terminal: WarpTerminal 
CPU: AMD Ryzen 7 3700X (16) @ 3.6GHz 
GPU: NVIDIA GeForce RTX 2070 SUPER 
Memory: 32 GiB
BIOS: American Megatrends Inc. 5.17 (07/07/2020)




### Platform

Ubuntu 24.04 amd64

### Version

uv 0.7.2

### Python version

_No response_

---

_Label `bug` added by @JWES1971 on 2025-05-07 12:48_

---

_Comment by @konstin on 2025-05-07 12:53_

Can you share your `pyproject.toml` and your `uv.lock`? You can reduce it to only include the minimal information to reproduce the stack overflow, so that you don't need to share too much of your private project. We need a package list that reproduces the stack overflow, otherwise we unfortunately can't reproduce it.

---

_Label `bug` removed by @konstin on 2025-05-07 12:53_

---

_Label `needs-mre` added by @konstin on 2025-05-07 12:53_

---

_Comment by @JWES1971 on 2025-05-07 12:57_

pyproject.toml:

[project]
name = "explore-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []

---

_Comment by @JWES1971 on 2025-05-07 13:01_

Results from uv lock --verbose

uv lock --verbose
DEBUG uv 0.7.2
DEBUG Found workspace root: `/home/jorge/projetos/cursos/explore-uv`
DEBUG Adding root workspace member: `/home/jorge/projetos/cursos/explore-uv`
DEBUG Reading Python requests from version file at `/home/jorge/projetos/cursos/explore-uv/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies the request: `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: explore-uv @ file:///home/jorge/projetos/cursos/explore-uv
DEBUG No workspace root found, using project root

thread 'uv-resolver' has overflowed its stack
fatal runtime error: stack overflow
Abortado (imagem do núcleo gravada)

---

_Comment by @konstin on 2025-05-07 17:22_

Are you using any other uv configuration files or does your machine have any other bespoke configuration or setup? It's surprising to me that a basically empty `pyproject.toml` would cause a stack overflow.

---

_Comment by @charliermarsh on 2025-07-11 02:15_

Closing, but can re-open if we receive an MRE.

---

_Closed by @charliermarsh on 2025-07-11 02:15_

---

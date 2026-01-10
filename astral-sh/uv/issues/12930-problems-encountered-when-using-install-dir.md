---
number: 12930
title: "Problems encountered when using `--install-dir`."
type: issue
state: closed
author: pansy1110
labels:
  - question
assignees: []
created_at: 2025-04-17T03:00:35Z
updated_at: 2025-04-17T12:53:00Z
url: https://github.com/astral-sh/uv/issues/12930
synced_at: 2026-01-10T01:25:27Z
---

# Problems encountered when using `--install-dir`.

---

_Issue opened by @pansy1110 on 2025-04-17 03:00_

### Question

I tried to install `Python 3.13.3` using the `uv python install` command and specified a custom installation path of `E:/languages/uv_python` through the `--install-dir` parameter. However, after the installation was completed, when I ran the `uv python list` command, I couldn't see the installed version of `Python 3.13.3`.

![Image](https://github.com/user-attachments/assets/a58757b8-0a5e-4c6b-a679-700fb4baaef1)

I checked the `E:/languages/uv_python` directory and found that the files of Python 3.13.3 really exist.

![Image](https://github.com/user-attachments/assets/a6e122e4-b84b-4599-b250-97e07f0b3336)

Running "E:\languages\uv_python\cpython-3.13.3-windows-x86_64-none\python.exe --version" manually can correctly output the version number.

![Image](https://github.com/user-attachments/assets/0e5d79e0-3f9b-4e01-97b4-5528f99e739b)

### Platform

Windows10 64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @pansy1110 on 2025-04-17 03:00_

---

_Comment by @j178 on 2025-04-17 06:26_

Use `--install-dir` to install to a non-default uv managed python installation directory, uv will not able to find it or manage it there, you can set `UV_PYTHON_INSTALL_DIR=E:\languages\uv_python` to help uv locate it.

---

_Comment by @pansy1110 on 2025-04-17 06:51_

> Use `--install-dir` to install to a non-default uv managed python installation directory, uv will not able to find it or manage it there, you can set `UV_PYTHON_INSTALL_DIR=E:\languages\uv_python` to help uv locate it.

@j178 Thank you. I tried but didn't succeed. Maybe my command was incorrect. I checked the official documentation but there was no example. Could you give me an example

---

_Comment by @j178 on 2025-04-17 10:25_

Can you try this?

```console
$env:UV_PYTHON_INSTALL_DIR='E:\languages\uv_python'
uv python list
```

---

_Comment by @charliermarsh on 2025-04-17 12:52_

Yeah I think you'd need to pass this to all commands (or set it as an env var so it's available persistently). uv needs to know where to look _after_ installing.

---

_Closed by @charliermarsh on 2025-04-17 12:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-17 12:53_

---

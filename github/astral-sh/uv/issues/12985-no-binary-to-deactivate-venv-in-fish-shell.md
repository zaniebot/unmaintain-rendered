---
number: 12985
title: No binary to deactivate venv in fish shell
type: issue
state: closed
author: Qazalbash
labels:
  - question
assignees: []
created_at: 2025-04-20T06:22:08Z
updated_at: 2025-04-20T15:16:22Z
url: https://github.com/astral-sh/uv/issues/12985
synced_at: 2026-01-07T13:12:18-06:00
---

# No binary to deactivate venv in fish shell

---

_Issue opened by @Qazalbash on 2025-04-20 06:22_

### Question

Hi, I am trying to use the virtual environment provided by uv. For context, I use fish shell and mamba. In mamba, `mamba deactivate` will deactivate the venv irrespective of the shell. In uv I saw there is a binary to activate the venv in fish shell, but only binary to deactivate is in `.bat` extension, which is not working on my platform. The only workaround for me is to kill the terminal. Is there any other solution to it?

```bash
(myvenv) <.venv/bin/(git:main)> ls -l | awk '{print $9}'
activate
activate.bat
activate.csh
activate.fish
activate.nu
activate.ps1
activate_this.py
deactivate.bat
pydoc.bat
python
python3
python3.12
```

### Platform

Linux 6.1.0-33-amd64 x86_64 GNU/Linux

### Version

uv 0.6.14

---

_Label `question` added by @Qazalbash on 2025-04-20 06:22_

---

_Comment by @FishAlchemist on 2025-04-20 14:39_

No, you misunderstood. Fish shell's ``deactivate`` command is loaded into the shell after running ``activate.fish``, which is why it doesn't have a standalone script.
https://github.com/astral-sh/uv/blob/e9e4ad4d7d96a80bc20ad2efaa9aaab646414171/crates/uv-virtualenv/src/activator/activate.fish#L38-L77

---

_Comment by @Qazalbash on 2025-04-20 15:16_

That's helpful! Thanks!

---

_Closed by @Qazalbash on 2025-04-20 15:16_

---

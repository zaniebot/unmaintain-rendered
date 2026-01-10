---
number: 15787
title: "Integration with VS Code \"Python Environments\" extension"
type: issue
state: open
author: schlich
labels:
  - enhancement
assignees: []
created_at: 2025-09-11T12:26:12Z
updated_at: 2025-09-13T17:13:21Z
url: https://github.com/astral-sh/uv/issues/15787
synced_at: 2026-01-10T01:25:59Z
---

# Integration with VS Code "Python Environments" extension

---

_Issue opened by @schlich on 2025-09-11 12:26_

### Summary

uv is not listed as an option in the setting for either "Default env manager" or "default package manager" in the new "Python Environments" VS Code extension:

<img width="535" height="220" alt="Image" src="https://github.com/user-attachments/assets/5e7192eb-80e0-4030-b7e1-5ab35663e99d" />

This is causing issues with third-party extensions that rely on the Python extension for various operations.

### Example

In the GitHub Copilot extension, you may configure agents to use the extensions for basic tool calls:

<img width="592" height="332" alt="Image" src="https://github.com/user-attachments/assets/5d81d4a5-e7d8-4ec0-87d0-7be96ce5b0b7" />

This defaults to pip, and it's unclear if it's possible to configure it to use uv, especially considering that the Python Environment extension appears to be specifically looking for other extensions:

<img width="498" height="263" alt="Image" src="https://github.com/user-attachments/assets/3b73aa3c-303e-4acc-a225-be974adaa741" />

---

_Label `enhancement` added by @schlich on 2025-09-11 12:26_

---

_Comment by @zanieb on 2025-09-11 12:45_

My understanding is that uv is transparently used if pip is selected? e.g., see https://github.com/microsoft/vscode-python-environments/issues/809

cc @eleanorjboyd

---

_Comment by @zanieb on 2025-09-11 12:45_

(Note this follows a discussion in [Discord](https://discord.com/channels/1039017663004942429/1415660422308040745))

---

_Comment by @zanieb on 2025-09-11 12:46_

Separately from that question, a "Python Environments" VS Code extension for uv is also tracked in https://github.com/astral-sh/uv/issues/11314

---

_Comment by @eleanorjboyd on 2025-09-12 17:30_

yes that is correct @zanieb it is used in the background! @karthiknadig thinking maybe we open an issue on the python envs repo and gather some feedback on if what makes sense for `uv` short term. I also got some pushback on https://github.com/microsoft/vscode-python-environments/issues/809 from others like @brettcannon since it feels that doesn't achieve what we want out of the experience and would be more confusing then helpful.

---

_Comment by @schlich on 2025-09-12 17:47_

Ok, thanks @eleanorjboyd .  So does uv use venv under the hood? @zanieb?  I thought it used virtualenv, but maybe that was just for an early release.... maybe that was part of my confusion.  Definitely could be hallucinating that memory.

---

_Comment by @zanieb on 2025-09-12 17:56_

uv implements virtual environment creation natively, it doesn't use venv or virtualenv.

---

_Comment by @jtkiley on 2025-09-13 17:13_

This extension is helpful, and having better `uv` support would be nice.

As it stands, you can add a package to the environment via the extension GUI, and it will disappear the next time `uv sync` is run. It's obvious when you know it's using `pip` and not updating `pyproject.toml`. However, it's a hard-to-understand beginner trap, because it'll work and then at some point it won't. In devcontainers, it'll happen in every rebuild or `uv sync`.

I talked about it in context in https://github.com/astral-sh/uv/issues/12197#issuecomment-3224624792, too.

Having package installs from the GUI work like you'd expect would be really nice for beginners.   It's really nice using `uv add` and having important packages recorded and versions pinned via the lock file automatically (compared to `pip install ...`, `pip freeze`, copy-paste to `requirements.txt`, and edit down to important packages). Having all that be true via the GUI would be even easier for beginners, who are often initially wary of the terminal.

What I don't know is whether this all also applies when VS Code suggests installing a package, but I would think so (e.g., paraphrasing, "Hey, you need `ipykernel` if you want to use Jupyter notebooks. Do you want me to install it?").

---

_Referenced in [astral-sh/uv#15960](../../astral-sh/uv/issues/15960.md) on 2025-09-20 16:16_

---

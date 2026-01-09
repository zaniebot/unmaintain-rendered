---
number: 2333
title: Use UV without VENV (devcontainer)
type: issue
state: closed
author: osbyrne
labels:
  - question
assignees: []
created_at: 2024-03-10T12:24:47Z
updated_at: 2024-06-05T12:35:03Z
url: https://github.com/astral-sh/uv/issues/2333
synced_at: 2026-01-07T13:12:17-06:00
---

# Use UV without VENV (devcontainer)

---

_Issue opened by @osbyrne on 2024-03-10 12:24_

> uv pip install manim_automata

error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.

my dev env is a devcontainer, specifically the image [mcr.microsoft.com/devcontainers/python:1-3.12-bullseye](mcr.microsoft.com/devcontainers/python:1-3.12-bullseye) hosted at [https://github.com/devcontainers/images/blob/main/src/python](https://github.com/devcontainers/images/blob/main/src/python)

I think .venv is useful to contain python dev envs, but in my case, the whole OS is contained. I shouldn't have to use .venv

---

_Comment by @charliermarsh on 2024-03-10 12:47_

Thanks, this is already supported -- you can pass `--system` or a specific Python interpreter with `--python path/to/python`. See the docs here: https://github.com/astral-sh/uv?tab=readme-ov-file#installing-into-arbitrary-python-environments.

---

_Closed by @charliermarsh on 2024-03-10 12:47_

---

_Label `question` added by @charliermarsh on 2024-03-10 12:47_

---

_Comment by @Malix-Labs on 2024-06-05 12:15_

But wait, [`--system` doesn't work in GitHub codespaces](https://github.com/astral-sh/uv/issues/3417)

---

_Comment by @zanieb on 2024-06-05 12:33_

If you don't have permissions to write to the system (as in GitHub codespaces) you should just create a virtual environment.

You can track `--user` support (which would be an alternative) in https://github.com/astral-sh/uv/issues/2077

---

_Comment by @Malix-Labs on 2024-06-05 12:35_

Yep, I am already tracking that one

---

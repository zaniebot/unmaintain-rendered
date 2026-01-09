---
number: 8119
title: "Feature suggestion: Auto create of venv"
type: issue
state: closed
author: rafaeljcd
labels:
  - question
assignees: []
created_at: 2024-10-11T04:21:36Z
updated_at: 2024-10-14T14:31:32Z
url: https://github.com/astral-sh/uv/issues/8119
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature suggestion: Auto create of venv

---

_Issue opened by @rafaeljcd on 2024-10-11 04:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments

> When running a command that mutates an environment such as uv pip sync or uv pip install, uv will search for a virtual environment in the following order:
> An activated virtual environment based on the VIRTUAL_ENV environment variable.
> An activated Conda environment based on the CONDA_PREFIX environment variable.
> A virtual environment at .venv in the current directory, or in the nearest parent directory.
>
> If no virtual environment is found, uv will prompt the user to create one in the current directory via uv venv.

According to the docs, this is the current flow of environment discovery. Instead of prompting user to create a venv, it would automatically create venv in order to skip some steps and save developer time.

---

_Comment by @charliermarsh on 2024-10-14 14:31_

We do automatically create it for the "project" APIs (`uv run`, `uv sync`, etc.), but not for the "pip" APIs (`uv pip install`, etc.). I think that's the right call -- it would be too surprising to users to automatically create it in the "pip" API commands which are intended to be closer to existing `pip` workflows.

---

_Label `question` added by @charliermarsh on 2024-10-14 14:31_

---

_Closed by @charliermarsh on 2024-10-14 14:31_

---

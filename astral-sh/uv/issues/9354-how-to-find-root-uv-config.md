---
number: 9354
title: How to find root uv config?
type: issue
state: closed
author: kevinsimper
labels:
  - question
assignees: []
created_at: 2024-11-22T13:59:49Z
updated_at: 2025-07-30T20:03:31Z
url: https://github.com/astral-sh/uv/issues/9354
synced_at: 2026-01-10T01:24:39Z
---

# How to find root uv config?

---

_Issue opened by @kevinsimper on 2024-11-22 13:59_

How does UV find root? What is the defining rule where UV stops looking?

https://docs.astral.sh/uv/concepts/projects/workspaces/#workspace-sources

turbo does it by defining it extends

<img width="283" alt="image" src="https://github.com/user-attachments/assets/f88f66e4-c3f8-4051-be17-f3e2880681cd">

https://turbo.build/repo/docs/reference/package-configurations

---

_Comment by @charliermarsh on 2024-11-22 14:02_

The root is the nearest parent directory containing a `pyproject.toml`.

---

_Label `question` added by @charliermarsh on 2024-11-22 14:03_

---

_Comment by @kevinsimper on 2024-12-02 13:54_

@charliermarsh But you can have multiple pyproject.toml? :)

https://docs.astral.sh/uv/concepts/projects/workspaces/#workspace-layouts

---

_Comment by @zanieb on 2024-12-02 22:29_

This is implemented at https://github.com/astral-sh/uv/blob/cf20673197f2073aa2ccde999423b816d3cb5ca5/crates/uv-workspace/src/workspace.rs#L125

We find the project root as the closest `pyproject.toml` then look for a workspace root in a parent directory https://github.com/astral-sh/uv/blob/cf20673197f2073aa2ccde999423b816d3cb5ca5/crates/uv-workspace/src/workspace.rs#L180

A parent `pyproject.toml` is only a workspace root if the project is a workspace member https://github.com/astral-sh/uv/blob/cf20673197f2073aa2ccde999423b816d3cb5ca5/crates/uv-workspace/src/workspace.rs#L1177-L1198

---

_Closed by @charliermarsh on 2024-12-26 16:55_

---

_Comment by @anthonyalayo on 2025-07-30 19:31_

@zanieb is there a way to make uv _**not**_ look at the parent directory? It's very unintuitive. I didn't realize that my parent directory also had a pyproject.toml, with a different python version. That made me scratch my head as to why my python version was wrong when trying to run uv commands in the directory that I care about.



---

_Comment by @zanieb on 2025-07-30 19:41_

Can you give more context about what your setup looks like? I'd recommend opening a new issue.

---

_Comment by @anthonyalayo on 2025-07-30 19:49_

@zanieb putting the following directory:

```
~/PycharmProjects/torch_test > tree                                                                                                               12:48:58 PM
.
├── main.py
├── pyproject.toml
├── README.md
└── uv.lock

1 directory, 4 files
```

Inside the following directory:
```
~/PycharmProjects/HuggingFaceGuidedTourForMac main !2 > tree                                                                                      12:49:26 PM
.
├── 00-SystemCheck.ipynb
├── 01-ChatBot.ipynb
├── 02-Benchmarks.ipynb
├── LegacyGuides
│   └── README_v3.md
├── LICENSE
├── NextSteps
│   ├── llama.cpp.md
│   └── Resources
│       ├── llama-model.png
│       └── llama2-chat.png
├── pyproject.toml
├── README.md
├── requirements.txt
├── Resources
│   ├── conda-envs.png
│   ├── huggingface-transformers.png
│   ├── jupyterlab.png
│   ├── no_venv.png
│   ├── ProjectFolder.png
│   └── venv_remind.png
└── uv.lock

5 directories, 18 files

```

Causes `uv` to use the `.python-version` from the parent directory instead of the current directory.

---

_Comment by @zanieb on 2025-07-30 19:57_

You can use `--no-config` to ignore that, or write a `.python-version` file in the child with your preference.

---

_Comment by @anthonyalayo on 2025-07-30 20:03_

There's a `.python-version` in the child as well, but that seems to be ignored:

```
~/Py/HuggingFaceGuidedTourForMac main !2 ?1 > ls -al
total 672
drwxr-xr-x  19 anthony  staff     608 Jul 30 13:01 .
drwxr-xr-x   7 anthony  staff     224 Jul 30 13:01 ..
drwxr-xr-x  12 anthony  staff     384 Jul 30 13:01 .git
-rw-r--r--   1 anthony  staff     113 Jul 29 23:19 .gitignore
drwxr-xr-x   3 anthony  staff      96 Jul 29 23:24 .ipynb_checkpoints
-rw-r--r--   1 anthony  staff       5 Jul 29 23:19 .python-version
drwxr-xr-x  10 anthony  staff     320 Jul 29 23:20 .venv
-rw-r--r--   1 anthony  staff    8586 Jul 29 23:28 00-SystemCheck.ipynb
-rw-r--r--   1 anthony  staff    3892 Jul 29 23:19 01-ChatBot.ipynb
-rw-r--r--   1 anthony  staff   11040 Jul 29 23:19 02-Benchmarks.ipynb
drwxr-xr-x   3 anthony  staff      96 Jul 29 23:19 LegacyGuides
-rw-r--r--   1 anthony  staff    1075 Jul 29 23:19 LICENSE
drwxr-xr-x   4 anthony  staff     128 Jul 29 23:19 NextSteps
-rw-r--r--   1 anthony  staff     469 Jul 30 12:18 pyproject.toml
-rw-r--r--   1 anthony  staff   20859 Jul 29 23:19 README.md
-rw-r--r--   1 anthony  staff      85 Jul 29 23:19 requirements.txt
drwxr-xr-x   8 anthony  staff     256 Jul 29 23:19 Resources
drwxr-xr-x   8 anthony  staff     256 Jul 30 12:31 torch_test
-rw-r--r--   1 anthony  staff  266747 Jul 29 23:19 uv.lock


~/Py/HuggingFaceGuidedTourForMac/torch_test main !2 ?1 > ls -al
total 112
drwxr-xr-x   8 anthony  staff    256 Jul 30 12:31 .
drwxr-xr-x  19 anthony  staff    608 Jul 30 13:01 ..
-rw-r--r--   1 anthony  staff      5 Jul 30 12:21 .python-version
drwxr-xr-x   9 anthony  staff    288 Jul 30 12:31 .venv
-rw-r--r--   1 anthony  staff     88 Jul 30 12:18 main.py
-rw-r--r--   1 anthony  staff    197 Jul 30 12:31 pyproject.toml
-rw-r--r--   1 anthony  staff      0 Jul 30 12:18 README.md
-rw-r--r--   1 anthony  staff  41549 Jul 30 12:31 uv.lock

~/Py/HuggingFaceGuidedTourForMac/torch_test main !2 ?1 > uv sync -v
DEBUG uv 0.8.3 (Homebrew 2025-07-24)
DEBUG Found project root: `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/torch_test`
DEBUG Found workspace root: `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac`
DEBUG Adding root workspace member: `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac`
DEBUG Adding discovered workspace member: `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/torch_test`
DEBUG Acquired lock for `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac`
DEBUG Reading Python requests from version file at `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/.python-version`
DEBUG Using Python request `3.12` from version file at `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/.python-version`
DEBUG Checking for Python environment at: `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.12`
DEBUG The project environment's Python version does not meet the Python requirement: `>=3.13`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/anthony/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.11-macos-aarch64-none`
DEBUG Found `cpython-3.12.11-macos-aarch64-none` at `/Users/anthony/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.11
DEBUG Released lock at `/var/folders/ny/15f9_7sj361dszmx90r1fhhr0000gn/T/uv-7d5badd5f2fb341a.lock`
error: The Python request from `/Users/anthony/PycharmProjects/HuggingFaceGuidedTourForMac/.python-version` resolved to Python 3.12.11, which is incompatible with the project's Python requirement: `>=3.13` (from workspace member `torch-test`'s `project.requires-python`).
Use `uv python pin` to update the `.python-version` file to a compatible version
```

 I'll go with this `--no-config`, thank you!

---

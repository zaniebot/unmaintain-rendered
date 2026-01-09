---
number: 8583
title: No toml changes when installing local package in editable mode
type: issue
state: closed
author: shanearcaro
labels:
  - question
assignees: []
created_at: 2024-10-26T03:05:18Z
updated_at: 2024-10-27T17:57:54Z
url: https://github.com/astral-sh/uv/issues/8583
synced_at: 2026-01-07T13:12:18-06:00
---

# No toml changes when installing local package in editable mode

---

_Issue opened by @shanearcaro on 2024-10-26 03:05_

 **I am on a M2 Mac on Sequoio 15.0,  uv version: uv 0.4.25 (97eb6ab4a 2024-10-21)
 The command is `uv pip install -e ../packages/jwt`**

Running the command with the `--verbose` flag:
`uv pip install -e ../packages/jwt --verbose`

DEBUG uv 0.4.25 (97eb6ab4a 2024-10-21)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/shane/Documents/Development/uv-test/backend/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.5 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG Requirement satisfied: file:///Users/shane/Documents/Development/uv-test/packages/jwt
Audited 1 package in 2ms
DEBUG Released lock at `/Users/shane/Documents/Development/uv-test/backend/.venv/.lock

When I run the uv pip install -e command, I get see uv has resolved and installed the package but my pyproject.toml file doesn't update. When I run `uv sync` it deletes the packages.

In my backend folder I have a dockerfile and a main.py file that starts a fastapi server. The docker file installs the python dependencies and uses additional context to load the local python dependencies. I have done the same process with pdm and local packages and I had no issues but I would like to switch to using uv.

This is the toml file
[project]
name = "backend"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "fastapi[all]>=0.115.3",
]

[tool.uv.sources]

---

_Comment by @zanieb on 2024-10-26 16:23_

`uv pip install` doesn't change your project definition. It sounds like you're looking for `uv add` instead.

---

_Label `question` added by @zanieb on 2024-10-26 16:23_

---

_Comment by @shanearcaro on 2024-10-27 17:57_

> `uv pip install` doesn't change your project definition. It sounds like you're looking for `uv add` instead.

Yup that was the issue. The error warning around the command is a little confusing though:
In pdm when adding an editable package you write `pdm add -e ./whatever` but with uv if you write the same command: `uv add -e ./whatever` you get:
 error: unexpected argument '-e' found
 tip: to pass '-e' as a value, use '-- -e' 
 
If you then try to fix this and write `uv add -- -e ./whatever` the error message is:
error: Failed to parse: `-e` Caused by: Expected package name starting with an alphanumeric character, found `-` -e
  
I found what I was looking for in the documentation, the correct command is `uv add --editable ./whatever` but the error messages were throwing me off.


---

_Closed by @shanearcaro on 2024-10-27 17:57_

---

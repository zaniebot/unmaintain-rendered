---
number: 13567
title: Shell command hashing causes global pip to be used after uv venv activation
type: issue
state: closed
author: Ramesh-kumar-S
labels:
  - question
assignees: []
created_at: 2025-05-21T07:44:53Z
updated_at: 2025-05-24T15:08:25Z
url: https://github.com/astral-sh/uv/issues/13567
synced_at: 2026-01-10T01:25:35Z
---

# Shell command hashing causes global pip to be used after uv venv activation

---

_Issue opened by @Ramesh-kumar-S on 2025-05-21 07:44_

### Summary

### Description:
When I create and activate a venv with uv venv, if I have previously used pip in the same shell, the shell (bash/zsh) may use the hashed global pip instead of the venv pip, even though $PATH is correct. Running hash -r fixes this, but it's not obvious to users.

This is not unique to UV, but it would be helpful if UV's documentation or CLI warned users about this common pitfall.

### Steps to Reproduce:
- Open a shell, run pip --version (uses global pip).
- Run uv venv .venv
- Activate the venv: source .venv/bin/activate
- Run pip --version (still uses global pip unless hash -r is run)

### Suggestion:
Please add a note to the documentation, or print a warning after venv activation, reminding users to run hash -r if they see this behavior.

### Platform

Linux 5.14.0-427.40.1.el9_4.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.17

### Python version

Python 3.13.3

---

_Label `bug` added by @Ramesh-kumar-S on 2025-05-21 07:44_

---

_Label `bug` removed by @konstin on 2025-05-21 09:03_

---

_Label `question` added by @konstin on 2025-05-21 09:03_

---

_Comment by @konstin on 2025-05-21 09:04_

This is not specific to uv, but a general behavior of shells such as bash. This problem does not occur with `uv run pip`, otherwise I don't think this is in scope for us to have special cases or documentation for the user's shell.

---

_Closed by @charliermarsh on 2025-05-24 15:08_

---

---
number: 16198
title: "uv tool python, error: externally-managed-environment"
type: issue
state: closed
author: GKTheOne
labels:
  - question
assignees: []
created_at: 2025-10-09T07:38:26Z
updated_at: 2025-10-10T01:12:41Z
url: https://github.com/astral-sh/uv/issues/16198
synced_at: 2026-01-10T01:26:04Z
---

# uv tool python, error: externally-managed-environment

---

_Issue opened by @GKTheOne on 2025-10-09 07:38_

### Question

I have python versions 3.12.11, 3.13.0, 3.13.4, and 3.14.0 installed.
I want to install the poetry tool using python 3.12 (13+14 have issues with emoji/ansi on my setup).
When I install poetry using `uv tool install poetry --python 3.12`, and then install/sync the environment, I end up with an `error: externally-managed-environment` (`This Python installation is managed by uv and should not be modified.`).
The odd thing here is that it appears to be using the project virtual environment 🤷
(`Command ['<redacted>\\AppData\\Local\\pypoetry\\Cache\\virtualenvs\\<redacted>-py3.12\\Scripts\\python.exe', '-m', 'pip', 'uninstall', 'pywin32', '-y'] errored with the following return code 1`)
However, when I delete all other version of python and only have python 3.12.11 installed before installing the poetry tool, the poetry install/sync of the environment works properly.

My question is (what) am I doing wrong/stupid? Before I make this a bug issue I'd like to know if it's just PBKAC?

### Platform

Windows 11

### Version

uv 0.9.0 (39b688653 2025-10-07)

---

_Label `question` added by @GKTheOne on 2025-10-09 07:38_

---

_Comment by @konstin on 2025-10-09 15:30_

> When I install poetry using uv tool install poetry --python 3.12, and then install/sync the environment

What do you mean with `then install/sync the environment`? Can you share the sequence of commands you ran and their output?

---

_Comment by @GKTheOne on 2025-10-10 01:12_

Apologies, I meant 'install/sync the project environment (via `poetry install` or `poetry sync`)'.

After further experimentation it turns out this is likely a [poetry (or virtualenv) bug #10490](https://github.com/python-poetry/poetry/issues/10490). It exists in poetry versions 2.1.4, 2.2.0, & 2.2.1 installed into my setup. Luckily poetry 2.1.3 does not have the problem.

---

_Closed by @GKTheOne on 2025-10-10 01:12_

---

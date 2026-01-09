---
number: 14312
title: Is it possible to cache isolated venvs like in script-mode?
type: issue
state: open
author: Kh4l3tH
labels:
  - question
assignees: []
created_at: 2025-06-27T11:21:40Z
updated_at: 2025-06-28T07:16:08Z
url: https://github.com/astral-sh/uv/issues/14312
synced_at: 2026-01-07T13:12:18-06:00
---

# Is it possible to cache isolated venvs like in script-mode?

---

_Issue opened by @Kh4l3tH on 2025-06-27 11:21_

### Question

# Intention
I have a python application stored on a **read-only** network share.
On any computer that can access this share, I only want to install `uv` and be able to run said application with the appropriate python version and dependencies.

# What I tried

### Dependencies as inline script metadata (PEP 723)
Simply running `uv run main.py` works like a charm!
Python + dependencies are installed and that environment is cached by default.

### Dependencies in pyproject.toml
Just running `uv run main.py` obviously fails because I don't have permission to create the `.venv` directory.
Running `uv run --isolated main.py` fails out of the box, because `uv` tries to create the `uv.lock` file.
After creating `uv.lock` manually, everything works, except that the environment is not cached, which gets pretty annoying if I run the script regularly.

# Question
Is there a way I can get the best of both worlds:
Can I get `uv` to create a local cached environment while working with the dependencies from the `pyproject.toml`? (Bonus: Without having to create the `uv.lock` file)

### Platform

Windows 11

### Version

0.7.15

---

_Label `question` added by @Kh4l3tH on 2025-06-27 11:21_

---

_Comment by @oconnor663 on 2025-06-27 16:03_

I'm not sure about how to customize the lockfile path, but for the `.venv` path have you seen [`UV_PROJECT_ENVIRONMENT`](https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path)?

---

_Comment by @Kh4l3tH on 2025-06-28 07:16_

No, but it looks like this would only work if I want to regularly run a **single** application on any computer?!

---

```yaml
number: 10813
title: How can I install project dependencies from an existing pyproject.toml?
type: issue
state: closed
author: alexted
labels:
  - question
assignees: []
created_at: 2025-01-21T13:16:27Z
updated_at: 2025-01-21T14:10:00Z
url: https://github.com/astral-sh/uv/issues/10813
synced_at: 2026-01-10T04:27:58Z
```

# How can I install project dependencies from an existing pyproject.toml?

---

_Issue opened by @alexted on 2025-01-21 13:16_

The official documentation doesn't clearly explain how to install project dependencies. It's all vague and indirect, with no concrete example/instructions!
How am I supposed to install project dependencies from an existing pyproject.toml? 
Where is that notorious `uv install` command that would install all the project packages?

---

_Comment by @FishAlchemist on 2025-01-21 13:35_

* ``uv pip install ``

https://docs.astral.sh/uv/pip/packages/#installing-packages-from-files
```bash
uv pip install -r pyproject.toml
```
* ``uv sync``

https://docs.astral.sh/uv/concepts/projects/sync/#creating-the-lockfile
If you run it directly as a project, then using uv sync should also work.

---

_Comment by @alexted on 2025-01-21 13:44_

Sorry, but who designed the CLI interface? What does pip have to do with it at all?
If you don’t mind a bit of criticism: the current UX of `uv` is astonishing in the worst possible way - everything is so counter-intuitive and confusing. My goodness, why?

---

_Comment by @FishAlchemist on 2025-01-21 13:53_

Actually, pip(24.3.1) can also install from pyproject.toml.

![Image](https://github.com/user-attachments/assets/3a086cf0-359e-439c-9a72-b6b4c8746beb)
I'm not sure about the UX, but it's definitely one of pip's features.

---

_Comment by @charliermarsh on 2025-01-21 13:59_

It sounds like you're looking for `uv sync`? You're being incredibly rude.

---

_Comment by @alexted on 2025-01-21 14:03_

Sorry for the rudeness, but I get really frustrated with unclear interfaces that are difficult to navigate and hard to figure out.

---

_Comment by @charliermarsh on 2025-01-21 14:06_

- `uv sync` will sync the project dependencies into the `.venv` in the project directory.
- `uv run {command}` will run a command in the project's environment, automatically locking and syncing if anything is out-of-date.
- `uv pip install` (and other `uv pip` commands) exist for compatibility with users that want a pip-style workflow. Commands like `uv sync` and `uv run` are project-centric, but much of the existing Python ecosystem is not. `uv pip` is an alternative interface to `uv sync`, `uv run`, etc. You would typically not use them together.

The project guide walks through adding dependencies, locking, syncing, and running commands: https://docs.astral.sh/uv/guides/projects/.

---

_Closed by @charliermarsh on 2025-01-21 14:06_

---

_Label `question` added by @charliermarsh on 2025-01-21 14:06_

---

_Comment by @alexted on 2025-01-21 14:09_

Thank you!

---

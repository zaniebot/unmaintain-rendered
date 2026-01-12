```yaml
number: 9246
title: "`ruff` disregards per file ignore on `E402`"
type: issue
state: closed
author: niccolomineo
labels:
  - question
assignees: []
created_at: 2023-12-22T15:02:39Z
updated_at: 2023-12-22T15:51:41Z
url: https://github.com/astral-sh/ruff/issues/9246
synced_at: 2026-01-12T15:54:49Z
```

# `ruff` disregards per file ignore on `E402`

---

_@niccolomineo_

For some reason the following directive is ignored:
```
"myproject/*sgi.py" = [
    "E402",
]
```
Issue:
```
$ ruff . --fix
myproject/wsgi.py:15:1: E402 Module level import not at top of file
Found 1 error.
$ ruff --version
ruff 0.1.9
```

Relevant settings:

```
[tool.ruff]
ignore = [
    "D203",
    "D212",
]
select = [
    "A",      # flake8-builtins
    "B",      # flake8-bugbear
    "C4",     # flake8-comprehensions
    "C90",    # McCabe cyclomatic complexity
    "D",      # pydocstyle
    "DJ",     # flake8-django
    "E",      # pycodestyle errors
    "F",      # Pyflakes
    "I",      # isort
    "Q",      # flake8-quotes
    "UP",     # pyupgrade
    "W",      # pycodestyle warnings
]
target-version = "py312"

[tool.ruff.per-file-ignores]
"myproject/*sgi.py" = [
    "E402",
]
"*/migrations/*.py" = [
    "D100",
    "D101",
    "D102",
    "D104",
]

[tool.ruff.isort]
known-first-party = [
    "myproject",
]
known-third-party = [
    "django",
]
```

---

_Renamed from "`ruff` ignores directive" to "`ruff` disregards per file ignore on `E402`" by @niccolomineo on 2023-12-22 15:03_

---

_Comment by @charliermarsh on 2023-12-22 15:06_

@niccolomineo - It looks like you're using `myproject` in the configuration file, but the directory on disk is called `project` ("project/wsgi.py:15:1: E402 Module level import not at top of file"). Could that be the culprit?

---

_Label `question` added by @charliermarsh on 2023-12-22 15:06_

---

_Comment by @niccolomineo on 2023-12-22 15:07_

> @niccolomineo - It looks like you're using `myproject` in the configuration file, but the directory on disk is called `project` ("project/wsgi.py:15:1: E402 Module level import not at top of file"). Could that be the culprit?

Sorry, double checked and it's my bad (I just failed at manually obfuscating the real dir name).

---

_Comment by @niccolomineo on 2023-12-22 15:17_

The issue unfortunately still stands.

---

_Comment by @niccolomineo on 2023-12-22 15:35_

@charliermarsh I see what the problem is. Those rules I reported in the opening post are in my `pyproject.toml`, but I also added a `ruff.toml` with the sole following rule:

```
[format]
docstring-code-format = true
```

I guess `ruff.toml` is overriding `pyproject.toml`. Do I need to move all `ruff` related rules currently in `pyproject.toml` to `ruff.toml`? I'd prefer to move the `docstring-code-format = true` rule only. If this is possible, what's the right syntax?

---

_Comment by @niccolomineo on 2023-12-22 15:40_

It looks like `tool.ruff.format` is correct.

---

_Closed by @niccolomineo on 2023-12-22 15:43_

---

_Comment by @charliermarsh on 2023-12-22 15:51_

👍 That's right!

---

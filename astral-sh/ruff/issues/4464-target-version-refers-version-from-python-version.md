```yaml
number: 4464
title: target-version refers version from .python-version file
type: issue
state: closed
author: jay0129
labels:
  - configuration
assignees: []
created_at: 2023-05-17T03:49:33Z
updated_at: 2023-05-17T15:33:24Z
url: https://github.com/astral-sh/ruff/issues/4464
synced_at: 2026-01-12T15:54:44Z
```

# target-version refers version from .python-version file

---

_@jay0129_

.python-version

My pyproject.toml look like this:
```
[tool.ruff]
# https://github.com/charliermarsh/ruff/blob/main/docs/configuration.md

line-length = 119

# Python 3.9.9
target-version = "py39"
```
My .python-version looks like this:
`3.9.9`

Is there a way for the file `pyproject.toml`  to refer version from `.python-version` ?

---

_Comment by @JonathanPlasse on 2023-05-17 06:01_

Currently, no.
It could be detected by `flake-to-ruff` for conversion from old settings but it is not standard so it should not use `.python-version` if `target-version` is not specified.
If you use `projet.require-python`, it will use it.
Black `line-length` is only use during conversion and not when using `ruff` directly as it is not standard as well.

---

_Comment by @charliermarsh on 2023-05-17 15:33_

Yeah, I can see the value but we don't currently support `.python-version` and I'm hesitant to do so because (1) it's not a standard, and (2) it then requires us to reconcile version information from multiple sources.

---

_Closed by @charliermarsh on 2023-05-17 15:33_

---

_Label `question` added by @charliermarsh on 2023-05-17 15:33_

---

_Label `configuration` added by @charliermarsh on 2023-05-17 15:33_

---

_Label `question` removed by @charliermarsh on 2023-05-17 15:33_

---

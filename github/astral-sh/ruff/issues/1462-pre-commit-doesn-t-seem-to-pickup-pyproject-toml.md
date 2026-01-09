---
number: 1462
title: "pre-commit doesn't seem to pickup pyproject.toml setting for line-length"
type: issue
state: closed
author: carlthome
labels:
  - question
assignees: []
created_at: 2022-12-30T03:25:06Z
updated_at: 2022-12-30T03:39:52Z
url: https://github.com/astral-sh/ruff/issues/1462
synced_at: 2026-01-07T13:12:14-06:00
---

# pre-commit doesn't seem to pickup pyproject.toml setting for line-length

---

_Issue opened by @carlthome on 2022-12-30 03:25_

I have a `pyproject.toml` with

```
[tool.black]
line-length = 100

[too.ruff]
line-length = 100
```
and a `.pre-commit-config.yaml` with
```
  - repo: https://github.com/psf/black
    rev: 22.12.0
    hooks:
      - id: black
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.0.200
    hooks:
      - id: ruff
```

Yet when running `pre-commit run --all` I'm getting

```
E501 Line too long (89 > 88 characters)
```
from `ruff`.

Have I set something up wrong or is this intended behavior?

---

_Comment by @charliermarsh on 2022-12-30 03:27_

No this looks correct to me. Where is the `pyproject.toml` in relation to the reported file?

---

_Label `question` added by @charliermarsh on 2022-12-30 03:27_

---

_Comment by @Jackenmen on 2022-12-30 03:28_

There's a typo in `too.ruff` (instead of `tool.ruff`)

---

_Comment by @carlthome on 2022-12-30 03:28_

Yikes, just noticed my little oops 😅

<img width="168" alt="image" src="https://user-images.githubusercontent.com/1595907/210031443-8500ce99-afb3-4ab0-a19f-1be74e59e729.png">

Now how to lint the pyproject.toml 🙉 

Thanks for the super quick response @charliermarsh!

---

_Closed by @carlthome on 2022-12-30 03:28_

---

_Comment by @charliermarsh on 2022-12-30 03:39_

Lol it got past me too! :)

---

_Referenced in [astral-sh/ruff#2899](../../astral-sh/ruff/issues/2899.md) on 2023-02-14 20:03_

---

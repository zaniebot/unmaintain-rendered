---
number: 12643
title: How to use uv with pyenv?
type: issue
state: closed
author: elcolie
labels:
  - question
assignees: []
created_at: 2025-04-03T05:40:55Z
updated_at: 2025-04-12T17:24:08Z
url: https://github.com/astral-sh/uv/issues/12643
synced_at: 2026-01-07T13:12:18-06:00
---

# How to use uv with pyenv?

---

_Issue opened by @elcolie on 2025-04-03 05:40_

### Question

Refer to this issue https://github.com/astral-sh/uv/issues/9088
`pyenv` is making `.python-version`, but it is not play well with `uv`.
How can I run `uv` with `pyenv`?

### Platform

OSX 15.3.2 M2

### Version

uv 0.6.12 (e4e03833f 2025-04-02)

---

_Label `question` added by @elcolie on 2025-04-03 05:40_

---

_Comment by @zanieb on 2025-04-03 13:06_

You're using `pyenv-virtualenv`?

---

_Comment by @elcolie on 2025-04-04 04:27_

Not sure about behind the hood. I normally use `pyenv virtualenv 3.12 my_env` to create. It might be `pyenv-virtualenv` you said.

---

_Comment by @zanieb on 2025-04-04 14:01_

Yeah we don't support working with `pyenv virtualenv`. We're considering something like https://github.com/astral-sh/uv/issues/12605 to improve compatibility

---

_Comment by @elcolie on 2025-04-12 17:24_

Thank you

---

_Closed by @elcolie on 2025-04-12 17:24_

---

---
number: 13186
title: How to replace pyenv global environment?
type: issue
state: closed
author: etienne-monier
labels:
  - question
assignees: []
created_at: 2025-04-28T19:59:31Z
updated_at: 2025-05-04T12:43:42Z
url: https://github.com/astral-sh/uv/issues/13186
synced_at: 2026-01-10T01:25:29Z
---

# How to replace pyenv global environment?

---

_Issue opened by @etienne-monier on 2025-04-28 19:59_

### Question

Hi,

A small question about my moving from pyenv to uv.

uv solves a lot the pyenv machinerie to maintain several python versions. Yet, I used it to bring a non-system python global environment when typing `python` and `pip install` outside of a project (say home directory).

This is quite different from tool management as, let's say I want a simple python interpreter (which is not the system one) with pip-installable packages (like ipython, click, etc).

What's the correct way to do this in uv?

Thanks for the help! 

### Platform

Ubuntu 20.04

### Version

_No response_

---

_Label `question` added by @etienne-monier on 2025-04-28 19:59_

---

_Comment by @konstin on 2025-04-28 20:30_

uv doesn't support global environments, but one option is to use `uv run --with <package1> --with <package2> python` to get an environment with those packages.

---

_Closed by @charliermarsh on 2025-05-04 12:43_

---

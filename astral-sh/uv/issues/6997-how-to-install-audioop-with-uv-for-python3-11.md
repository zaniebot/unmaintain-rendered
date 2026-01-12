```yaml
number: 6997
title: How to install audioop with uv for python3.11 ?
type: issue
state: closed
author: munishkhatri720
labels:
  - question
assignees: []
created_at: 2024-09-04T07:42:40Z
updated_at: 2024-09-14T00:43:42Z
url: https://github.com/astral-sh/uv/issues/6997
synced_at: 2026-01-12T15:59:09Z
```

# How to install audioop with uv for python3.11 ?

---

_@munishkhatri720_

uv Version : `uv 0.4.2 (736ccb950 2024-09-01)`
Python Version : 3.11.8 
Platform : Windows 10

I am trying to install [`discord.py`](<https://github.com/Rapptz/discord.py>) from git using uv but I am getting this issue 
```py
 × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.11) does not satisfy Python>=3.13 and audioop-lts==0.2.1 depends on Python>=3.13, we can  
      conclude that audioop-lts==0.2.1 cannot be used.
      And because only audioop-lts==0.2.1 is available and your project depends on audioop-lts, we can conclude that your project's       
      requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.11) includes Python versions that are not supported by your dependencies (e.g.,
      audioop-lts==0.2.1 only supports >=3.13). Consider using a more restrictive `requires-python` value (like >=3.13).
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
  ```
"I know `audioop` is deprecated since Python 3.11, but someone is maintaining [`audioop`](https://github.com/AbstractUmbra/audioop) for future versions. My question is, how can I install this package via `uv` from their Git repository? Their README only provides examples for Poetry, Pip, etc., not for `uv`."

---

_Comment by @zanieb on 2024-09-04 13:00_

So.. audioop-lts declares a _minimum Python version requirement_ of 3.13 https://github.com/AbstractUmbra/audioop/blob/7f415a8a591473653679a005581638cd3f203b3e/pyproject.toml#L8

Since your project declares that it supports Python >=3.11, audioop-lts can't be used because it can't be installed on Python 3.11 or 3.12. You'll need to increase the minimum Python version of your project to 3.13 (as suggested in the error message) or open an issue on audioop-lts to support more Python versions.

---

_Label `question` added by @zanieb on 2024-09-04 13:00_

---

_Comment by @apollo13 on 2024-09-09 06:20_

The audioop docs also suggest to add it via a `python_version>='3.13'` constraint.

---

_Closed by @charliermarsh on 2024-09-14 00:43_

---

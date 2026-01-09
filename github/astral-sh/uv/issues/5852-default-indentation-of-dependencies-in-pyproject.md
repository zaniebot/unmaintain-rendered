---
number: 5852
title: "Default indentation of dependencies in `pyproject.toml`"
type: issue
state: open
author: my1e5
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-08-07T12:28:12Z
updated_at: 2025-09-04T13:02:12Z
url: https://github.com/astral-sh/uv/issues/5852
synced_at: 2026-01-07T13:12:17-06:00
---

# Default indentation of dependencies in `pyproject.toml`

---

_Issue opened by @my1e5 on 2024-08-07 12:28_

While it is not explicitly stated, the PyPA guide to the `pyproject.toml` file uses two spaces for the default indentation of TOML arrays - see https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-and-requirements. uv is currently using four spaces. 

I know that uv is able to respect the existing indentation level in the `pyproject.toml` file (see https://github.com/astral-sh/uv/issues/5009) which is nice. 

Nevertheless, I wanted to pose this as an open question as to what the default setting should be. 

(The same discussion from Rye - https://github.com/astral-sh/rye/issues/1078)

---

_Comment by @zanieb on 2024-08-07 12:45_

I think it should be changed to two.

---

_Comment by @paduszyk on 2024-12-18 08:31_

I am upvoting for two spaces as well.

Alternatively, uv could respect `.editorconfig` if available.


---

_Referenced in [spear-ai/citizen#553](../../spear-ai/citizen/issues/553.md) on 2025-04-01 22:40_

---

_Comment by @ssbarnea on 2025-09-04 11:26_

Any chance to see it implemented? Switching to two should be quite easy to achieve. I also observed that uv adds trailing comma to the last array element, something that my current formatter removes, but I would be ok to reconfigure it to add them just to keep up with uv.

Another alternative would be allow execution of a formatter command after changing the file, so the formatter would fix the file to match current rules.

---

_Label `enhancement` added by @zanieb on 2025-09-04 13:02_

---

_Label `help wanted` added by @zanieb on 2025-09-04 13:02_

---

_Referenced in [cookiecutter/cookiecutter-django#6074](../../cookiecutter/cookiecutter-django/pulls/6074.md) on 2025-09-23 17:10_

---

_Referenced in [astral-sh/uv#16006](../../astral-sh/uv/pulls/16006.md) on 2025-09-23 19:04_

---

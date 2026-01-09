---
number: 13013
title: "How to use `uv add` to install libraries without installing my project"
type: issue
state: open
author: Quang-elec44
labels:
  - question
assignees: []
created_at: 2025-04-21T03:32:31Z
updated_at: 2025-04-23T14:25:40Z
url: https://github.com/astral-sh/uv/issues/13013
synced_at: 2026-01-07T13:12:18-06:00
---

# How to use `uv add` to install libraries without installing my project

---

_Issue opened by @Quang-elec44 on 2025-04-21 03:32_

### Question

I tried to install new libraries using `uv add` but it installed my project too. Is there any flag in `uv add` to avoid this?

### Platform

_No response_

### Version

uv 0.6.6 (c1a0bb85e 2025-03-12)

---

_Label `question` added by @Quang-elec44 on 2025-04-21 03:32_

---

_Comment by @zanieb on 2025-04-21 18:26_

Why don't you want the project to be included? You can set `tool.uv.package = False` in your `pyproject.toml` to avoid including your project.

---

_Comment by @Quang-elec44 on 2025-04-22 04:54_

@zanieb Here is my problem:
For end users, the package should be included. However, for developers contributing to the source code, every time I add a package (e.g., langchain), it rebuilds and reinstalls the source code into the virtual environment.

---

_Comment by @zanieb on 2025-04-23 14:25_

What's wrong with that though?

---

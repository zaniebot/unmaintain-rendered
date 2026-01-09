---
number: 7940
title: Project environments docs have a bit of a contradiction
type: issue
state: closed
author: vincentdavis
labels:
  - documentation
assignees: []
created_at: 2024-10-05T12:15:15Z
updated_at: 2024-10-05T15:02:05Z
url: https://github.com/astral-sh/uv/issues/7940
synced_at: 2026-01-07T13:12:17-06:00
---

# Project environments docs have a bit of a contradiction

---

_Issue opened by @vincentdavis on 2024-10-05 12:15_

`uv creates a virtual environment in a .venv directory next to the pyproject.toml.`

Reading the above line I think that using `uv init --app MyApp` will create a virtual environment. But it does not.

https://github.com/astral-sh/uv/blob/79555f3e678e33cd85d34ee4ec133bfbdfd05ad4/docs/concepts/projects.md?plain=1#L272

A bit further down, there is this line. This is when the environment is created.

`When `uv run` is invoked, it will create the project environment if it does not exist.`

https://github.com/astral-sh/uv/blob/79555f3e678e33cd85d34ee4ec133bfbdfd05ad4/docs/concepts/projects.md?plain=1#L281C1-L281C86


---

_Renamed from "Project environments docs have a bit of a contridiction" to "Project environments docs have a bit of a contradiction" by @vincentdavis on 2024-10-05 12:15_

---

_Label `documentation` added by @AlexWaygood on 2024-10-05 12:28_

---

_Referenced in [astral-sh/uv#7941](../../astral-sh/uv/pulls/7941.md) on 2024-10-05 15:01_

---

_Closed by @zanieb on 2024-10-05 15:02_

---

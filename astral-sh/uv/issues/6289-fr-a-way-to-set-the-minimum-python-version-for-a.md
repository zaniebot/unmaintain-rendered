---
number: 6289
title: "FR: A way to set the minimum python version for a virtual workspace"
type: issue
state: closed
author: ilyagr
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-08-21T00:33:30Z
updated_at: 2024-09-09T00:54:01Z
url: https://github.com/astral-sh/uv/issues/6289
synced_at: 2026-01-10T01:23:58Z
---

# FR: A way to set the minimum python version for a virtual workspace

---

_Issue opened by @ilyagr on 2024-08-21 00:33_

I set up a virtual workspace to run MkDocs as discussed [here](https://github.com/astral-sh/rye/discussions/1342#discussioncomment-10401062); the config is the same is in #6288 (which I worked around by not running `uv lock`).

A minor annoyance is that `uv lock` sets `requires-python = ">=3.12"` in `uv.lock`, and there is no way to decrease this requirement AFAICT. `uv` seems to only accept a field for the minimum Python version in *packages*, and there are none in my config.

Perhaps the `[tool.uv.workspace]` section in `pyproject.toml` should support setting a minimum python version, or there should be a way to have a "virtual project" that has dev-dependencies and sets a minimum Python version instead of having to create a weird empty workspace to put dev-dependencies in its root.


**Update:** Ah, looking at #6285, maybe I'm supposed to have a `.python-version`... OTOH, judging by that bug, it may or may not work.

---

_Comment by @charliermarsh on 2024-08-21 00:35_

Yeah this is an awkward spot right now. The best you can do at present is a `.python-version`.

---

_Renamed from "FR: A way to set the python version for a virtual workspace" to "FR: A way to set the minimum python version for a virtual workspace" by @ilyagr on 2024-08-21 00:40_

---

_Label `enhancement` added by @charliermarsh on 2024-08-21 02:10_

---

_Label `configuration` added by @charliermarsh on 2024-08-21 02:10_

---

_Comment by @charliermarsh on 2024-09-09 00:42_

This is now solved by using a virtual package. You can add `[project]` with a Python version to what was previously a virtual workspace root.

---

_Closed by @charliermarsh on 2024-09-09 00:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 00:42_

---

_Comment by @ilyagr on 2024-09-09 00:54_

Thank you!

---

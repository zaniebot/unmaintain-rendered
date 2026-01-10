---
number: 7729
title: "Must the executable name match package name when using `uv tool run package`"
type: issue
state: closed
author: gitgithan
labels:
  - question
assignees: []
created_at: 2024-09-27T05:20:16Z
updated_at: 2024-09-28T19:10:22Z
url: https://github.com/astral-sh/uv/issues/7729
synced_at: 2026-01-10T01:24:18Z
---

# Must the executable name match package name when using `uv tool run package`

---

_Issue opened by @gitgithan on 2024-09-27 05:20_

Command ran: `uv tool run toolong`  
Output: 
```
The executable `toolong` was not found.
warning: An executable named `toolong` is not provided by package `toolong`.
The following executables are provided by `toolong`:
- tl
```
https://github.com/Textualize/toolong

I believe the issue is uv is expecting the executable name (tl) to be the same as package name (toolong).
**Is a workaround like below necessary?**

It can work with `uv run --with toolong -- tl file.log` though after getting inspiration from sample at https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run. 
**Is this a workaround or i'm not using `uv tool run` and `uv run` properly?**

**Why did `uv tool run` api get invented if `uv run` looks like it covers the same things that uv tool run does?**

I noticed `uv run` has --python to download python version on demand but `uv tool` does not have --python. 
**Does this mean when using `uv tool` the python we want must already exist and be set as default?** 

I wish the docs had an explicit summary table/diagram on what is being automated for each API. 
- python download
- venv creation
- library installation

Currently i'm reading in docs and ctrl+f/eyeballing keywords to find this information. 

uv platform: WSL2 
uv version: uv 0.4.15


---

_Comment by @charliermarsh on 2024-09-27 12:18_

The intended use is `uv tool run --from toolong tl`. `uv tool run` (`uvx`) and `uv run` serve different purposes. The former installs an executable for global use (like `pipx`), while the latter runs a command within a project. So `uv run` will have access to any dependencies defined in a `pyproject.toml`, while `uv tool run` can be used anywhere to run a dedicated executable.

---

_Comment by @charliermarsh on 2024-09-27 12:18_

`uv tool run` does support `--python`, as in `uv tool run --python 3.10 --from toolong tl`.

---

_Label `question` added by @charliermarsh on 2024-09-27 12:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-27 12:18_

---

_Comment by @zanieb on 2024-09-27 12:57_

There's some more background on the tool interface in #3560 

---

_Closed by @charliermarsh on 2024-09-28 19:10_

---

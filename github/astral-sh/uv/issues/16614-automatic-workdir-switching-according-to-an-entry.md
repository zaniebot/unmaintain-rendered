---
number: 16614
title: "Automatic workdir switching according to an entry point's target"
type: issue
state: open
author: Dev-iL
labels:
  - question
assignees: []
created_at: 2025-11-06T12:58:12Z
updated_at: 2025-11-06T18:18:28Z
url: https://github.com/astral-sh/uv/issues/16614
synced_at: 2026-01-07T13:12:19-06:00
---

# Automatic workdir switching according to an entry point's target

---

_Issue opened by @Dev-iL on 2025-11-06 12:58_

### Question

This is part question, part feature request.

My use case is a monorepo containing multiple streamlit dashboards organized in their own sub-modules. Each dashboard has its own `.streamlit` folder containing secrets and configurations. My goal is to allow a user to `uv run my_dashboard` from the monorepo root and have it boot the right dashboard wherever it may be nested. When I attempted to define the below in the topmost `pyproject.toml`
```toml
[project.scripts]
my_dashboard = "my_package.my_dashboard.main:main"
```
it was possible to run the dashboard, but it did not pick up the contents of the `.streamlit` folder, indicating that the working directory was set incorrectly, likely to the folder we ran the `uv run` command from. Indeed, upon switching to the dashboard's main folder and running it from there - the secret loading problem was no longer present.

I saw the `--project` and `--directory` flags mentioned in the docs, but these require the user to specify the folder to switch to - which is undesired in this use case. 

**Question**: Is there a way to switch the working directory automatically to where the entry point points? 

As I user, I would expect the auto-folder-switching behavior to be configurable at the `pyproject.toml` for each entry point. I have considered the approach of a flag such as `--autocwd` for `uv run`, but the main downside of this is the user might not know they need to add it.

---------
P.S.
streamlit usually doesn't work through `uv run`, but there are [workarounds](https://github.com/streamlit/streamlit/issues/9450).

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Dev-iL on 2025-11-06 12:58_

---

_Comment by @zanieb on 2025-11-06 17:50_

Do you control loading the `.streamlit` folder or running the invocation that does? Or does it happen behind the scenes?

---

_Comment by @Dev-iL on 2025-11-06 18:07_

 Behind the scenes afaik. I believe [this](https://github.com/streamlit/streamlit/blob/44175886ce6411e4863fc36206aab10d180cb70e/lib/streamlit/file_util.py#L69) (file) is where the path resolution happens.

Are you suggesting to try and switch the workdir inside the python code before the .streamlit folder is loaded? That's an interesting idea!

---

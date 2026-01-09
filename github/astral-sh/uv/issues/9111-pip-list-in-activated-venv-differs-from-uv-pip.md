---
number: 9111
title: "`pip list` in activated venv differs from `uv pip list`"
type: issue
state: closed
author: nataziel
labels:
  - question
assignees: []
created_at: 2024-11-14T03:22:16Z
updated_at: 2024-11-14T06:05:44Z
url: https://github.com/astral-sh/uv/issues/9111
synced_at: 2026-01-07T13:12:18-06:00
---

# `pip list` in activated venv differs from `uv pip list`

---

_Issue opened by @nataziel on 2024-11-14 03:22_

This may be working as intended but is extremely confusing.

I understand that `uv` creates a virtual environment called `.venv` by default when first adding requirements to a project, or running `uv sync` or `uv venv` etc.

If you are in the project folder but don't have the venv activated, when you run the command `uv pip list` it will list the packages installed in that venv, but running `pip list` will list the packages in your base environment. This makes sense.

By extension, say I have `pytest` installed in the venv, but not my base python installation, if I run `pytest`, pytest won't be on $PATH and the command will fail. If I run `uv run pytest` it works as expected, again this makes sense.

When you activate that venv by running `source .venv/Scripts/activate`, you can now run `pytest` and it will work, which again matches my expectations. 

However, if you run `pip list` with the venv activated it will list the packages installed in your base python installation, not the activated venv. If you had created the venv via `python -m venv .venv`, and activated it exactly the same way, `pip list` would list the packages in the activated venv, not the base python installation. This creates a very confusing situation where `pip list` and `uv pip list` don't agree on what packages are available.

What I think it all boils down to is the presence of `pip` in the venv.

Some experimentation has found that this doesn't happen if you create the venv with `python -m venv .venv`, then use `uv sync` to install the project packages (`pip` is available within the activated venv!). This only works if the python version specified in `pyproject.toml` and `.python-version` match the base python installation. 

If they don't match, `uv` destroys the existing `.venv/` directory and creates a new one. The venv created by `uv` doesn't have `pip`, so if you run `pip list` it uses the `pip` from the base environment. Therefore, it lists different packages to the what is listed by `uv pip list`

Is this expected behaviour? If it is, is there a way it can be made less confusing? Even just showing a warning that the listed packages refer to a different environment than the activated venv would make this way more clear. 


---

_Comment by @charliermarsh on 2024-11-14 03:33_

All expected. We don't include pip in the virtual environment, so running `pip list` is calling the pip installed on your system.

---

_Comment by @charliermarsh on 2024-11-14 04:52_

(You can run `uv venv --seed` if you want to include pip in a uv-created virtualenv. We don't include those "seed packages" by default.)

---

_Label `question` added by @charliermarsh on 2024-11-14 05:03_

---

_Comment by @nataziel on 2024-11-14 06:03_

Ok yep understood. Is there a way you could intercept the call to `pip` and print a warning of that surprising behaviour?

Also, you may want to consider a `uv list` or similar command to display the project dependencies. `uv tree` is good but sometimes a bit verbose. I found myself running `cat pyproject.toml` quite a bit while I was experimenting with adding and removing dependencies

---

_Closed by @nataziel on 2024-11-14 06:03_

---

_Referenced in [astral-sh/uv#14215](../../astral-sh/uv/issues/14215.md) on 2025-06-25 23:34_

---

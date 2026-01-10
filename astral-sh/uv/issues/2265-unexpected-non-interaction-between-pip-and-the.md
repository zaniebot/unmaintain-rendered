---
number: 2265
title: Unexpected non-interaction between pip and the virtual environment from uv
type: issue
state: closed
author: grjzwaan
labels: []
assignees: []
created_at: 2024-03-07T07:04:26Z
updated_at: 2024-03-07T14:16:47Z
url: https://github.com/astral-sh/uv/issues/2265
synced_at: 2026-01-10T01:23:15Z
---

# Unexpected non-interaction between pip and the virtual environment from uv

---

_Issue opened by @grjzwaan on 2024-03-07 07:04_

I created a virtual environment with `uv`, activated it and then I used `pip list -u` without thinking: the results were unexpected. It took me a while to figure out what was going wrong.

*Somehow* in my mind `uv pip != pip`, but the created virtual environment would work the same. As in, I could use `pip` in it. I can make a guess on why this makes sense, but it still took me off guard.

Other than that `uv` was *very nice* to work with.

# Reproduction

Commands:
```
uv venv
.venv\Scripts\activate
pip list -u  # <--- Checks global Python packages and not in the virtual env
```

Similarly:
```
uv venv
.venv\Scripts\activate
pip install numpy  # <--- Installs in global Python, not in the virtual env
```
# Possible resolution
Perhaps it's good to mention this in the readme? For example, in the getting started add a sentence about not mixing uv pip and pip, nor expect the virtual env to be exactly the same as the virtual environment created with `venv`.

In the virtual environment created by uv you could intercept pip commands and print a warning, but for me documentation would have been sufficient.


# Version & OS
Windows 11 on uv 0.1.14 (c525fdf2b 2024-03-04)

---

_Comment by @charliermarsh on 2024-03-07 14:05_

Hey! One additional note that might be helpful: if you run with `uv venv --seed`, we'll include `pip` in the virtualenv -- we just don't do it by default.

---

_Comment by @charliermarsh on 2024-03-07 14:16_

I'm gonna merge this into https://github.com/astral-sh/uv/issues/1330.

---

_Closed by @charliermarsh on 2024-03-07 14:16_

---

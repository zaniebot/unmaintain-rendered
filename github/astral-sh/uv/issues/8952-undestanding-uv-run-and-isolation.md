---
number: 8952
title: Undestanding uv run and isolation
type: issue
state: closed
author: Levelleor
labels:
  - question
assignees: []
created_at: 2024-11-08T18:33:06Z
updated_at: 2024-11-08T18:52:12Z
url: https://github.com/astral-sh/uv/issues/8952
synced_at: 2026-01-07T13:12:18-06:00
---

# Undestanding uv run and isolation

---

_Issue opened by @Levelleor on 2024-11-08 18:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I found out that when running bash script uv explicitly triggers a creation of a .venv in a project: 
```bash
$ uv run echo "Hello!"
Creating virtual environment at: .venv
```

But when I run a python script with declarative dependency definitions it seems to ignore this step:
```python
# /// script
# requires-python = "==3.10"
# dependencies = [
#   "click",
# ]
# ///

print("Hello World!")
```
the output of `uv run -vvv example.py` in this case produces some entries that seem relevant:
```
...
uv_virtualenv::virtualenv Ignoring empty directory
...
        uv_install_wheel::linker::link_wheel_files
Installed 1 package in 1ms
 + click==8.1.7
...
Hello World!
```

I am assuming uv does create a temporary virtual environment in this case, however it's not totally evident from the logs.

Questions:
1) Is it safe to have different scripts use different versions of Python and different versions of same packages?
2) Why does invoking a bash script create a ".venv"? Shouldn't those, basically, also only rely on a temporary venv (if that's the case for scripts)?
3) If I set require-python to a version of python not allowed by the `.python-version` file it seems to throw a warning about an unsupported version of python requested. Dropping `.python-version` file seems to resolve the issue and force uv into installing the necessary version for a script, would there be any downside in doing so?

---

_Comment by @zanieb on 2024-11-08 18:44_

> Is it safe to have different scripts use different versions of Python and different versions of same packages?

Yes

> Why does invoking a bash script create a ".venv"? Shouldn't those, basically, also only rely on a temporary venv (if that's the case for scripts)?

Invoking `uv run` in a project creates an environment for the project. Scripts with inline metadata are always run in an environment isolated from the project.

> If I set require-python to a version of python not allowed by the .python-version file it seems to throw a warning about an unsupported version of python requested. Dropping .python-version file seems to resolve the issue and force uv into installing the necessary version for a script, would there be any downside in doing so?

You can update the `.python-version` file with `uv python pin`. You can also drop the file. The downside is that now someone else working on the project may not use the same Python version by default — they'll use whatever the first version compatible with the `requires-python` specifier is on their system.

---

_Label `question` added by @zanieb on 2024-11-08 18:44_

---

_Comment by @Levelleor on 2024-11-08 18:52_

Thanks a lot!

---

_Closed by @Levelleor on 2024-11-08 18:52_

---

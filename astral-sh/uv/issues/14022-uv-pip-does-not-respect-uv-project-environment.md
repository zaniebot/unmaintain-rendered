```yaml
number: 14022
title: "`uv pip` does not respect $UV_PROJECT_ENVIRONMENT"
type: issue
state: open
author: xoolive
labels:
  - question
  - needs-decision
assignees: []
created_at: 2025-06-13T12:47:03Z
updated_at: 2025-07-17T20:34:22Z
url: https://github.com/astral-sh/uv/issues/14022
synced_at: 2026-01-12T16:01:42Z
```

# `uv pip` does not respect $UV_PROJECT_ENVIRONMENT

---

_@xoolive_

### Summary

Not sure whether this behaviour is by design, but it looks confusing to me.

It happens that I just want to quickly add a package to virtualenv for a specific use case, and I am perfectly fine with it being trashed during the next `uv sync`

```shell
$ cd foo
$ export UV_PROJECT_ENVIRONMENT=$HOME/foo/.venv2
$ uv sync  # creates a .venv2 folder for the environment
$ uv run python  # uses the .venv2 environment
$ uv pip install numpy  # this one basically expects the .venv folder
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

### Platform

not platform specific

### Version

uv 0.5.21

### Python version

Python 3.13.0 

---

_Label `bug` added by @xoolive on 2025-06-13 12:47_

---

_Label `bug` removed by @zanieb on 2025-06-13 12:51_

---

_Label `question` added by @zanieb on 2025-06-13 12:51_

---

_Comment by @zanieb on 2025-06-13 12:52_

This is intentional at the moment, `uv pip` is entirely agnostic of the project settings. I understand why it'd be nice for it to respect it here, but I worry it'd be confusing too.

We could consider respecting `UV_PROJECT_ENVIRONMENT` if `VIRTUAL_ENV` is unset 🤔 hm

---

_Label `needs-decision` added by @zanieb on 2025-06-13 12:52_

---

_Comment by @xoolive on 2025-06-13 12:55_

Thanks for quick head-up, and good luck for taking the better decision :)


---

_Comment by @joshdunnlime on 2025-06-13 15:10_

I have found this for `UV_SYSTEM_PYTHON` also.

---

_Comment by @chuckn246 on 2025-07-17 20:34_

Somewhat related, I don't think `uv python find` respects `UV_PROJECT_ENVIRONMENT` either:
```bash
# Set the variable
$ export UV_PROJECT_ENVIRONMENT="$HOME/Projects/uv_testing/venv"

# Create project
$ uv init my_project
Initialized project `my-project` at `/Users/chuckn246/Projects/uv_testing/my_project`

# Change directories
$ cd my_project

# Create venv at UV_PROJECT_ENVIRONMENT
$ uv venv
Using CPython 3.13.3
Creating virtual environment at: /Users/chuckn246/Projects/uv_testing/venv
Activate with: source /Users/chuckn246/Projects/uv_testing/venv/bin/activate

# Prints incorrect location
$ uv python find
/Users/chuckn246/.local/share/uv/python/cpython-3.13.3-macos-aarch64-none/bin/python3.13

# Unset the variable
$ unset UV_PROJECT_ENVIRONMENT

# Create venv in project root
$ uv venv
Using CPython 3.13.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

# Prints correct location
$ uv python find
/Users/chuckn246/Projects/uv_testing/my_project/.venv/bin/python3

# List of directories
$ ls -l ../
total 0
drwx------ 9 chuckn246 staff 288 Jul 17 16:27 my_project
drwx------ 7 chuckn246 staff 224 Jul 17 16:27 venv
```

---

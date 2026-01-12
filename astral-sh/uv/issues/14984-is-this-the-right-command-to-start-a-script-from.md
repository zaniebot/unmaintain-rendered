```yaml
number: 14984
title: Is this the right command to start a script from within its own dedicated project venv?
type: issue
state: open
author: torchss
labels:
  - question
assignees: []
created_at: 2025-07-31T01:20:33Z
updated_at: 2025-07-31T01:29:10Z
url: https://github.com/astral-sh/uv/issues/14984
synced_at: 2026-01-12T16:02:01Z
```

# Is this the right command to start a script from within its own dedicated project venv?

---

_@torchss_

### Question

**TLDR**: `#(ve3)/ve3: uv run --offline --project /script/ /script/script.py <params>` ?

I'm developing a script `/script/script.py` that has its own dedicated project venv under `/script/.venv`. I don't want:

a. `uv` to create either implicit or temporary venvs for `script.py` because I already have exactly what I need under `/script/.venv`.
b. to install the script globally/systemwide. (`uv` itself however, is installed globally/systemwide.)

This script is used to operate on _other_ python projects that might have their own project venv under `$PWD/.venv` but I don't want `uv` to even read or attempt to load those orthogonal venvs during those invocations.

**For example**: Given that I'm at `/ve3/` which is a Python project with it's own, active venv under `/ve3/.venv`, I would like to launch `/script/script.py` while passing it some parameters `<params>`.

Is this the best and correct way:

`#(ve3)/ve3: uv run --offline --project /script/ /script/script.py <params>`

I realize `--offline` could be redundant but I put that in as a "paranoid option" in case `uv`, for some reason decided to update packages for `script.py` or the project at CWD as I only want `uv` to invoke the script without `uv` affecting any changes. If this fear is completely misplaced, let me know so I can remove this flag.

Further questions:

1. Does the command `uv run --python /script/.venv /script/script.py <params>` literally use the specific python executable under /script/.venv without also activating the venv available to `script.py`? This would mean `--python` isn't the behavior I need for my use-case?
2. When using this command, `uv` shows this:

```
warning: `VIRTUAL_ENV=.venv` does not match the project environment path `/ve3/.venv` and will be ignored; use `--active` to target the active environment instead
```

Is there a way to suppress only this specific message? I would like to view any and all other messages
3. If `/script/script.py` was my own personal fork of [aider](https://github.com/Aider-AI/aider), would you recommend a different way of running it?

### Platform

LINUX

### Version

`uv 0.6.3`

---

_Label `question` added by @torchss on 2025-07-31 01:20_

---

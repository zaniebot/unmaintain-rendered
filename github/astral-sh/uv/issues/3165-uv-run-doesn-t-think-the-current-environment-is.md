---
number: 3165
title: "`uv run` doesn't think the current environment is compatible when it is [rye virtual package issue?]"
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2024-04-21T13:16:09Z
updated_at: 2024-07-09T19:05:20Z
url: https://github.com/astral-sh/uv/issues/3165
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv run` doesn't think the current environment is compatible when it is [rye virtual package issue?]

---

_Issue opened by @bluss on 2024-04-21 13:16_

```
uv 0.1.35
platform: linux (x86_64)
```

Problem: uv run always creates a new environment even if current should be compatible, in certain projects.

Hi, trying out `uv run` for https://github.com/bluss/pyproject-local-kernel purposes (it is a thing that uses "rye run python", "poetry run python", "pdm run python" etc as appropriate depending on configuration).

I know that `uv run` is not an official feature yet, still it's fun to try to see how it works.

### How to Reproduce

1. git clone -b 0.5.5 https://github.com/bluss/pyproject-local-kernel
2. cd pyproject-local-kernel/tests/server-client/client
3. Inspect [pyproject.toml](https://github.com/bluss/pyproject-local-kernel/blob/main/tests/server-client/client/pyproject.toml) and requirements files to see they only depend on ipykernel and jinja2.
4. uv venv --python 3.11.7    # it's the python I happened to use
5. uv pip install -r requirements.lock
6. uv run -v python

Expected: It uses the environment ./.venv in the current directory
Actual behaviour: `DEBUG Using Python 3.11.7 environment at .uv/.tmpvVFPMZ/bin/python`


Note: Using python 3.12.2 instead has the same result.
Note: Using uv pip install -r pyproject.toml instead of the requirements.lock file has the same result.
Note: If I remove the dependencies in the pyproject.toml and reinstall, it still fails. So it might be about the client "package" defined by the pyproject.toml itself?  That might be the cause, according to rye this is not a package at all, because it's marked virtual and should not be installed. (I don't know how uv is supposed to know that or if there's a rye agnostic way to configure this..)

In general I wanted here that if there was a complete "rye sync"'ed environment, it should be used as is by uv run if possible.

---

_Renamed from "`uv run` doesn't think the current environment is compatible when it is" to "`uv run` doesn't think the current environment is compatible when it is [rye virtual package issue?]" by @bluss on 2024-04-21 13:28_

---

_Comment by @zanieb on 2024-04-21 15:12_

Thanks for giving it a try. This seems weird, I'd imagined this would work. I'll look into this.

---

_Assigned to @zanieb by @zanieb on 2024-04-21 15:12_

---

_Label `bug` added by @zanieb on 2024-04-21 15:12_

---

_Comment by @zanieb on 2024-07-09 14:27_

Is this still an issue?

---

_Comment by @bluss on 2024-07-09 19:05_

This seems to be fixed. Uv has developed a lot since then, anyway. In this particular case it doesn't use an emphemeral venv anymore, so that's good.

---

_Closed by @bluss on 2024-07-09 19:05_

---

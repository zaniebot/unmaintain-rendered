```yaml
number: 5257
title: Ctrl-C to to uv run is not handled by the child process
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2024-07-21T14:01:26Z
updated_at: 2024-07-24T12:33:11Z
url: https://github.com/astral-sh/uv/issues/5257
synced_at: 2026-01-10T04:53:49Z
```

# Ctrl-C to to uv run is not handled by the child process

---

_Issue opened by @bluss on 2024-07-21 14:01_

uv 0.2.27
platform: linux (x86_64)

Jupyterlab wants to handle Ctrl-C but it doesn't get a chance to do it when started from uv run.

Steps to reproduce

1. uv init jptest
2. cd jptest
3. uv run --with jupyterlab jupyter-lab
4. Wait 3 seconds
5. Ctrl-C

Jupyterlab prints something like this

```
Shut down this Jupyter server (y/[n])? [W 2024-07-21 15:57:14.911 LabApp] Could not determine jupyterlab build status without nodejs
[I 2024-07-21 15:57:19.915 ServerApp] No answer for 5s:
[I 2024-07-21 15:57:19.915 ServerApp] resuming operation...
```

No input reaches jupyterlab after the Ctrl-C, so it doesn't get an answer to its y/[n] question.

Depending on circumstances jupyterlab is either continuing to run or shutting down in the background. `uv run` seems to terminate itself and leave its child process running.

Both jupyterlab and uv run receive the signal? In the Expected behaviour, jupyterlab should handle this signal alone (and it does so using the prompt in the output)?

I think a good solution to this would also solve the ipykernel launch problem mentioned in #3095.

`rye run` handles this one as expected. `pdm run` doesn't let jupyter-lab handle the signal but at least jupyter-lab is not left running detached.

Wish would be that it works the same way as if the user is running `.venv/bin/jupyter-lab`, if that's possible.

---

_Renamed from "Ctrl-C to to uv run is not reaching the process" to "Ctrl-C to to uv run is not handled by the child process" by @bluss on 2024-07-21 14:02_

---

_Comment by @bluss on 2024-07-21 14:31_

Also worth experimenting with just the Python REPL the same way. Less dependencies, similar problem.

Compare how this one handles Ctrl-C:

> uv run python

compared with:

> python

---

_Label `bug` added by @zanieb on 2024-07-21 14:52_

---

_Closed by @charliermarsh on 2024-07-24 12:33_

---

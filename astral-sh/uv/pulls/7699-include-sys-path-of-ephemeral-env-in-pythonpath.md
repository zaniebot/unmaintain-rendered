```yaml
number: 7699
title: Include sys.path of ephemeral env in PYTHONPATH
type: pull_request
state: closed
author: tfsingh
labels: []
assignees: []
draft: true
base: main
head: tfsingh/jupyter
created_at: 2024-09-26T08:35:00Z
updated_at: 2024-10-01T22:33:58Z
url: https://github.com/astral-sh/uv/pull/7699
synced_at: 2026-01-10T12:53:53Z
```

# Include sys.path of ephemeral env in PYTHONPATH

---

_Pull request opened by @tfsingh on 2024-09-26 08:35_

This PR ensures that the sys.path of an ephemeral environment is included in the PYTHONPATH of spawned processes (useful in the case of executing external commands). This addresses issue #7647.

I tested this with the repro in the issue, i.e.:

```
uv init
uv add jupyter
uv run --with pydantic jupyter lab
```

An automated test is trickier — Jupyter doesn't have an interface for executing commands non-interactively (closest thing is ```jupyter console```, which spawns a repl). Its runtime (ipython) does accept a ```-c``` flag, but the issue doesn't manifest when using ipython — as such,  a test wouldn't be effective. Open to thoughts on more rigorous test plans, I might be missing something!

---

_Renamed from "Include sys.path of ephemeral env in PYTHONPATH of external command" to "Include sys.path of ephemeral env in PYTHONPATH" by @tfsingh on 2024-09-26 08:35_

---

_Marked ready for review by @tfsingh on 2024-09-26 08:45_

---

_Comment by @zanieb on 2024-09-26 12:26_

You might just be able to do something like `python -c "import sys; print(sys.path)"`?

---

_Comment by @charliermarsh on 2024-09-26 12:50_

A few issues with this that come to mind though I'm not certain on either:

1. I _think_ entries in `PYTHONPATH` have slightly different precedence / prioritization than they would if you run `uv run` here without a notebook. You can probably see the difference if you print out `sys.path` within the notebook and with `uv run --with pydantic python -c "import sys; print(sys.path)"`.
2. I _think_ that using `PYTHONPATH` won't follow `.pth` files though @carljm would know -- in which case, we wouldn't be able to resolve any `--with-editable` dependencies added to the `uv run` command.

---

_Comment by @bluss on 2024-09-26 16:16_

I'm concerned that setting PYTHONPATH could have an impact on `uv run` inside `uv run` and so on (which becomes more common as uv gets more useful to run everything!). When setting PYTHONPATH there is risk that python environments bleed into each other and packages are imported between environments that should be separate.

This was issue #5459 in the past.

----

You have two environments, Base and Layer.

The root cause of the problem is that `Base/bin/jupyter` is a python script that hard-codes that it runs using the `Base/bin/python` interpreter.

Could a solution be to execute `Layer/bin/python Base/bin/jupyter`? That would activate the correct environment.

---

_Comment by @charliermarsh on 2024-09-26 16:21_

I've tried `uv run --with pydantic python -m jupyter lab` and it doesn't work despite using the layered Python :/

---

_Comment by @bluss on 2024-09-26 16:22_

I just discovered that. :cry:  jupyter lab must be executing the jupyter-lab script stub?

---

_Comment by @charliermarsh on 2024-09-26 16:22_

Yeah there's something going on that I don't understand yet.

---

_Comment by @bluss on 2024-09-26 16:25_

And if I "fix" the jupyter-lab script stub to just use usr/bin/env python, a different error is produced. This is not going to play very well with environment layering, because jupyterlab is searching for its $prefix/share/jupyter/lab

```
JupyterLab Error
JupyterLab application assets not found in "/home/user/.cache/uv/archive-v0/5CUs8Rso39JXZO4nfVoDF/share/jupyter/lab"
Please run `jupyter lab build` or use a different app directory
```

---

_Comment by @tfsingh on 2024-09-26 16:46_

> Could a solution be to execute Layer/bin/python Base/bin/jupyter? That would activate the correct environment.

This was the first thing I tried as well, using the ephemeral env's interpreter to execute ```Base/bin/jupyter``` — I'll need to dig deeper here to understand this.

> A few issues with this that come to mind though I'm not certain on either:

Both of these points make sense as well (verified the first, looking into the second now)


---

_Converted to draft by @zanieb on 2024-10-01 19:38_

---

_Comment by @tfsingh on 2024-10-01 22:33_

Going to close this as I'm not actively working on it, will reopen if I figure out a better approach

---

_Closed by @tfsingh on 2024-10-01 22:33_

---

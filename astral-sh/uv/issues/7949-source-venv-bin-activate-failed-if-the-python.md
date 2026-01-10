---
number: 7949
title: source .venv/bin/activate failed if the python version is not already installed by pyenv
type: issue
state: open
author: songron
labels: []
assignees: []
created_at: 2024-10-06T22:08:03Z
updated_at: 2024-10-06T22:20:11Z
url: https://github.com/astral-sh/uv/issues/7949
synced_at: 2026-01-10T01:24:21Z
---

# source .venv/bin/activate failed if the python version is not already installed by pyenv

---

_Issue opened by @songron on 2024-10-06 22:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I use pyenv to manage python versions on my computer (apple m1). 
The problem is that if the specified python version was not already installed by pyenv, `source .venv/bin/activate` wouldn't activate the virtualenv (and reported no errors).

How to reproduce: 
For example, if I have python 3.12.5 but not 3.12.6 available in pyenv. If I do

```
uv init -p 3.12.6
uv add ipython
source .vene/bin/activate  # does nothing
```
It **wouldn't activate the virtual env**. Though it was actually created and python 3.12.6 was added by uv. `uv run` worked well.

Instead, if I start with `uv init -p 3.12.5` the issue was gone. The only difference is that `3.12.5` was already installed previously by pyenv.




---

_Comment by @songron on 2024-10-06 22:20_

It's interesting that if I do install the desired python with python (pyenv install 3.12.6), then it works --- `source .venv/bin/activate` will activate the virtual env created by uv. Though the venv python activated was actually managed by uv (linked to `~/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12`) and has nothing to do with pyenv. 

It's confusing to me why it failed to activate venv at the first place since the later installed python by pyenv was not used in the virtual env in anyway (I suppose).

---

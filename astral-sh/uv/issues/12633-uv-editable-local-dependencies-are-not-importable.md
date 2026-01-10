---
number: 12633
title: UV editable local dependencies are not importable via python
type: issue
state: closed
author: risky-rickman
labels:
  - bug
assignees: []
created_at: 2025-04-02T17:25:17Z
updated_at: 2025-04-03T01:15:37Z
url: https://github.com/astral-sh/uv/issues/12633
synced_at: 2026-01-10T01:25:22Z
---

# UV editable local dependencies are not importable via python

---

_Issue opened by @risky-rickman on 2025-04-02 17:25_

### Summary

When i run the following commands in zsh i would expect to be able to import my lib package while running the python interpreter in my project .venv., however i get an importError.   Perhaps i am doing something wrong.  Because of this, i am unable to use UV to support python monorepos.
```
mkdir projects
mkdir shared
cd projects
uv init project
cd ../shared
uv init lib
cd ../projects
cd project
 uv add --editable ../../shared/lib
uv sync
source .venv/bin/activate
python
import lib 
```

### Platform

MacOS Version 15.2 (24C101), Apple M2 Pro

### Version

uv 0.6.11

### Python version

Python 3.13.2

---

_Label `bug` added by @risky-rickman on 2025-04-02 17:25_

---

_Comment by @charliermarsh on 2025-04-02 17:45_

Try adding the `--lib` flag to your `uv init` invocations.

---

_Comment by @risky-rickman on 2025-04-02 18:03_

ah, that solved it!  Thank you so much!

---

_Closed by @risky-rickman on 2025-04-02 18:03_

---

_Comment by @charliermarsh on 2025-04-02 18:07_

No problem. We plan to make to enable that setting (or something like it) by default in the future to avoid this problem.

---

_Comment by @risky-rickman on 2025-04-02 18:24_

It appears that using the --lib flag forces me to have src/project under projects/project, and I can't modify this as the dependencies are (secretly, not declaratively in the pyproject.toml) linked this way.  

I feel like this is going to make managing a monorepo really cumbersome.  Shouldn't i be able to simply declare a folder to be used as a package?

---

_Comment by @charliermarsh on 2025-04-02 18:29_

I don't fully understand the layout of your project, but yes, what you're describing should be possible. You just need to configure the build backend for each of those packages (e.g., `hatchling`) to point to that directory.

---

_Comment by @risky-rickman on 2025-04-02 18:32_

alright, sounds like i just need to get more educated then.  I'll do some research.  Thanks for your help Charlie!

---

_Comment by @risky-rickman on 2025-04-02 23:09_

I'm almost finished with my setup, but it doesn't appear that the --lib option is configurable in the pyproject.toml.  This makes it impossible for me to use an existing pyproject.toml to create a usable environment with 'uv sync'.  Is this something I also need to configure via hatch?

---

_Comment by @zanieb on 2025-04-03 01:15_

`--lib` just creates a project layout. It's not a persistent setting. You should be able to have a usable environment with an existing `pyproject.toml`. Perhaps you can use the output of `uv init --lib` as an example? If your goal is still to have a flat structure (i.e., no `src/` directory), then yeah — you need to configure your build backend (hatchling, I presume).

---

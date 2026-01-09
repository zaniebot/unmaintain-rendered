---
number: 10691
title: "`uv pip install` makes warnings in installed packages appear that don't appear with `pip install`"
type: issue
state: closed
author: gerhard4
labels:
  - compatibility
assignees: []
created_at: 2025-01-16T17:45:22Z
updated_at: 2025-01-16T18:23:08Z
url: https://github.com/astral-sh/uv/issues/10691
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install` makes warnings in installed packages appear that don't appear with `pip install`

---

_Issue opened by @gerhard4 on 2025-01-16 17:45_

When I install my requirements into a virtual environment with `uv pip sync/install`, I get these warnings from `docopt` (version 0.6.2):
```
/home/.../ve/bin/python -m doctest --fail-fast -o ELLIPSIS <...>
/home/.../ve/lib/python3.12/site-packages/docopt.py:165: SyntaxWarning: invalid escape sequence '\S'
  name = re.findall('(<\S*?>)', source)[0]
...
```
But when I use `pip install` to install the same requirements (same `docopt` version), I don't get them.

I know why the warning is there, but it's library code and I don't want to get warnings for (working) library code. I also know that I can get rid of the warning with `PYTHONWARNINGS="ignore::SyntaxWarning"`, but I'd rather not have to use this in every environment I run Python code.

My virtual environment is in a directory "ve" of the project directory. The commands I use are
```
uv pip sync --python ve/bin/python requirements.txt  # I tried this first; it gives me the SyntaxWarnings.
uv pip install --python ve/bin/python -r requirements.txt  # Then I tried this; same result.
ve/bin/pip install -r requirements.txt  # When I use this, I don't get the SyntaxWarnings.
```

It doesn't matter whether I created the virtual environment with `uv` or standard Python (same Python version in both cases).

Is there something I can do to make `uv` behave the same as `pip` in this case?

---

_Comment by @zanieb on 2025-01-16 17:56_

I have no clue why you'd get this from uv but not pip 🤔 

These warnings are shown on usage after install?

---

_Label `compatibility` added by @zanieb on 2025-01-16 17:56_

---

_Comment by @charliermarsh on 2025-01-16 17:56_

We don't compile bytecode by default, while pip does. Once the bytecode is compiled, these warnings don't appear.

---

_Comment by @charliermarsh on 2025-01-16 17:57_

See, e.g., https://github.com/astral-sh/uv/issues/1559 or https://github.com/astral-sh/uv/issues/1928#issuecomment-1968857514.

---

_Comment by @charliermarsh on 2025-01-16 17:58_

It's a real warning, it's just hidden when you install via pip. You can set [`UV_COMPILE_BYTECODE`](https://docs.astral.sh/uv/configuration/environment/#uv_compile_bytecode) if you want to change the behavior.

---

_Closed by @charliermarsh on 2025-01-16 17:58_

---

_Comment by @gerhard4 on 2025-01-16 18:20_

> These warnings are shown on usage after install?

They show up on first usage. This is explained by charliermarsh's comments. 

And when I searched for issues, I mistakenly only searched open issues, or else I would have found the related issues Charlie posted. Sorry.

---

_Comment by @charliermarsh on 2025-01-16 18:23_

No worries. I’ll add something to the docs to make note of this. It was super confusing to us when we first saw it.

---

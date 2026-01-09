---
number: 15867
title: What is the point of uv python install?
type: issue
state: closed
author: elygeo
labels:
  - question
assignees: []
created_at: 2025-09-15T07:32:43Z
updated_at: 2025-12-03T04:01:00Z
url: https://github.com/astral-sh/uv/issues/15867
synced_at: 2026-01-07T13:12:19-06:00
---

# What is the point of uv python install?

---

_Issue opened by @elygeo on 2025-09-15 07:32_

### Question

The very first command in the uv docs is `uv python install`.
After running the command we have $HOME/.local/bin/python3.13.
We cannot add packages to it with `uv add, uv pip, or python3.13 -m pip`.
It is not necessary for `uv run`.
So what exactly is the use case for `uv python install`? I guess vanilla Python scripts with no external packages?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @elygeo on 2025-09-15 07:32_

---

_Comment by @konstin on 2025-09-15 08:05_

The Python version installed is used as basis for creating virtual environments (this is why you can't add packages to it, it's the clean base environment for creating venvs from). For example, `uv run` can use this Python interpreter to create `.venv` which it uses to run your code. If you don't call `uv python install` explicitly `uv run -p 3.13` can automatically download and install this Python version.

---

_Comment by @elygeo on 2025-09-15 13:00_

Thanks konstin.

The difference is that `uv python install` links executables into ~/.local/bin and complains if that is not on your PATH. So now this may be the default python you get from the command line, but I can't see a good reason for that. In fact it can cause problems. I guess it would be handy if you have many `python -m venv` in old setup scripts, but for new projects we can do `uv venv` or `uv sync` to create venvs.

So my question should more specifically be: why does `uv python install` link executables in ~/.local/bin? My takeaway is to never do `uv python install`.  I must be thinking about wrong if the very first command in the docs is already causing problems.

---

_Comment by @konstin on 2025-09-15 13:04_

This behavior was requested by users. There are different use cases for it, some like to use the python REPL of a plain Python, others need it to be in PATH, some like to have Python installation as separate CI step, see https://github.com/astral-sh/uv/issues/6265 for context.

---

_Comment by @konstin on 2025-09-15 13:07_

> So my question should more specifically be: why does `uv python install` link executables in ~/.local/bin? My takeaway is to never do `uv python install`. I must be thinking about wrong if the very first command in the docs is already causing problems.

It is totally fine to never manually run `uv python install` yourself or use the `python`/`python3.x` in `~/.local/bin`. Since installing Python was history complex and it's otherwise not clear that uv can manage Python versions, we find it valuable to document this command. The shim in `~/.local/bin` is only a small aspect of this feature, which was added much later than `uv python install`.

---

_Comment by @elygeo on 2025-09-15 13:41_

Got it, thanks!

If I could make a suggestion it would be that if you are going to have the shims, make it a venv and let me install packages there. Otherwise the shims don't seem useful and just add clutter. EDIT: I see from #6265 they are useful!

I have almost 20 years of python scripts scattered across my system and it's not practical to change all the shebangs to uv run. I am trying to fit uv to fit into how things have traditionally worked until I'm convinced to go all in. For now what I do is create a venv and put that on my PATH for my general purpose system python.

---

_Closed by @charliermarsh on 2025-12-03 04:01_

---

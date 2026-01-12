```yaml
number: 9157
title: Symlink uv itself into venv
type: issue
state: closed
author: rdaysky
labels:
  - question
  - wontfix
assignees: []
created_at: 2024-11-15T23:28:07Z
updated_at: 2024-12-26T16:57:37Z
url: https://github.com/astral-sh/uv/issues/9157
synced_at: 2026-01-12T15:59:43Z
```

# Symlink uv itself into venv

---

_@rdaysky_

The following workflow seems to have no uv counterpart:
```
python3 -m venv /path/to/venv
/path/to/venv/bin/pip install somepkg
/path/to/venv/bin/python -m somepkg
```
It would be nice to be able to run
```
uv venv /path/to/venv
/path/to/venv/bin/uv pip install somepkg
/path/to/venv/bin/python -m somepkg
```
where none of the advanced features of uv are required, only its speed.

---

_Comment by @zanieb on 2024-11-15 23:29_

Why not

```
uv venv /path/to/venv
uv pip install -p /path/to/venv somepkg
/path/to/venv/bin/python -m somepkg

---

_Comment by @charliermarsh on 2024-11-15 23:36_

You don’t need uv to be in the environment in order to manipulate the environment, which simplifies things quite a bit.

---

_Label `question` added by @zanieb on 2024-11-15 23:41_

---

_Comment by @rdaysky on 2024-11-16 22:25_

The main rationale for this request is simply the principle of least surprise. If pip can do something, then uv pip should be able to do the exact same thing.

To be specific, I suggest the following:
* that an executable called `uv` be put into all newly created virtualenvs
* that if called, it should discover the virtualenv in which it’s located and only affect that virtualenv
* that `uv` executables created by any method other than `uv venv`, such as `pipx install uv`, do _not_ do this, because otherwise, for example, uv from pipx would try to affect the virtualenv into which pipx installed it

How this compares to current behavior:
* it will be possible to execute /path/to/venv/bin/uv, currently that’s not possible
* given PATH=/path/to/venv/bin:$PATH, `uv` invocations will affect that venv, but the current behavior is also exactly the same as uv discovers virtualenvs from $PATH

How this can bite the user in the behind:
* if a symlink to uv is put into a virtualenv and later the uv binary moves to a different place which is still on $PATH, in which case the old way would work but the new one won’t. The same would happen with the python interpreter itself though, so virtualenv users are already aware of such a possibility.

Alternatives:
* put an executable called `pip` (or `uvpip` or something) that works exactly like normal pip would, including virtualenv discovery from the directory containing it, but that would call `uv pip` behind the scenes. This would enable the use case I mentioned in the issue (specifically, what I wanted was to install spacy and ipython to run some ad hoc stats on a piece of text, I had no interest in creating a pyproject.toml for a throwaway script).

Related concerns:
* on Windows there may be other reasons to have uv within the venv (see #3876)
* if uv reaches widespread adoption and distributions such as Debian, known for conservativeness with new versions of stuff, start packaging it, there may be need for system uv to download and install a more recent version of uv into a newly created venv

@charliermarsh One binary you definitely don’t need in the environment to make use of it is `python`, yet it’s there.

---

_Comment by @zanieb on 2024-11-16 22:27_

I'm sorry, but I'm not really convinced this is worth the additional complexity.

---

_Closed by @charliermarsh on 2024-12-26 16:57_

---

_Label `wontfix` added by @charliermarsh on 2024-12-26 16:57_

---

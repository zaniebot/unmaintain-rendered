```yaml
number: 10272
title: "Multiple problems with ` uv`. Related to mounted disk?"
type: issue
state: closed
author: ego-thales
labels:
  - needs-mre
assignees: []
created_at: 2025-01-02T14:35:06Z
updated_at: 2025-01-15T10:06:13Z
url: https://github.com/astral-sh/uv/issues/10272
synced_at: 2026-01-12T16:00:10Z
```

# Multiple problems with ` uv`. Related to mounted disk?

---

_@ego-thales_

Platform: Ubuntu 22.04
uv version: 0.5.13
---

Hello!

Not my first issue, but again, **thanks for the amazing work**!!
I'm trying to use `uv`  for a work project without success so far -- no problem on home pc.

###### Problems encountered

- I don't understand the inconsistency (says "not installed" but finds it later on)
```console
$ uv run mypy
uvpyenv: version `3.13.1' is not installed (set by path/to/my/project/.python-version)
$ uv python list
cpython-3.13.1+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.1-linux-x86_64-gnu                 ~/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3.13
cpython-3.12.8-linux-x86_64-gnu                 <download available>
cpython-3.11.11-linux-x86_64-gnu                <download available>
cpython-3.10.16-linux-x86_64-gnu                <download available>
cpython-3.10.12-linux-x86_64-gnu                /usr/bin/python3.10
cpython-3.10.12-linux-x86_64-gnu                /usr/bin/python3 -> python3.10
cpython-3.10.12-linux-x86_64-gnu                /bin/python3.10
cpython-3.10.12-linux-x86_64-gnu                /bin/python3 -> python3.10
cpython-3.9.21-linux-x86_64-gnu                 <download available>
cpython-3.8.20-linux-x86_64-gnu                 <download available>
cpython-3.7.9-linux-x86_64-gnu                  <download available>
pypy-3.10.14-linux-x86_64-gnu                   <download available>
pypy-3.9.19-linux-x86_64-gnu                    <download available>
pypy-3.8.16-linux-x86_64-gnu                    <download available>
pypy-3.7.13-linux-x86_64-gnu                    <download available>
$ uv run python
Python 3.13.1 (main, Dec 19 2024, 14:32:25) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

- `uv run pytest` runs for a while, no output, even with `-vv`. After a while, no report was generated so I don't think it silently worked, and the console juste goes back to normal as if nothing happened.
```console
$ uv run pytest
$ 
```

- `uv build` runs "forever", no output except this ont line
```bash
$ uv build
Building source distribution...
```

###### Worth mentioning

My project is located in a mounted disk (e.g. `~/mounted/myproject)`  which already encouraged me to use `--link-mode=symlink` in several places, mainly to `uv add`.


Thank you in advance if you can or try to help!
Cheers

---

_Comment by @ego-thales on 2025-01-02 14:38_

Also, I have `pyenv` installed, I used it together with `poetry`  on previous projects, and I'd like to only use `uv`  from now  on but I need to keep `pyenv` for older ones.

---

_Comment by @ego-thales on 2025-01-02 14:54_

Ok I got more info from `uv -v run pytest`.
```console
me@workstation:~/mounted/workspace/myproject$ uv -v run pytest
DEBUG uv 0.5.13
DEBUG Found project root: `~/mounted/workspace/myproject`
DEBUG No workspace root found, using project root
DEBUG Discovered project `myproject` at: ~/mounted/workspace/myproject
DEBUG Reading Python requests from version file at `~/mounted/workspace/myproject/.python-version`
DEBUG Using Python request `3.13.1` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.13.1`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: myproject @ file://~/mounted/workspace/myproject
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements

[...]  # Checking requirements

Audited 55 packages in 24ms
DEBUG Using Python 3.13.1 interpreter at: ~/mounted/workspace/myproject/.venv/bin/python3
DEBUG Running `pytest`
DEBUG Command exited with signal: Some(7)
```

Regarding `uv -v build`, it's stuck at
```console
DEBUG Calling `hatchling.build.build_sdist("~/mounted/workspace/myprojectdist", {})`
```

Regarding `uv -v run mypy`, it indeed seems related to `pyenv`.
```console
DEBUG Using Python 3.13.1 interpreter at: ~/mounted/workspace/myproject/.venv/bin/python3
DEBUG Running `mypy`
pyenv: version `3.13.1' is not installed (set by ~/mounted/workspace/myproject/.python-version)
DEBUG Command exited with code: 1
```

---

_Comment by @konstin on 2025-01-03 09:58_

What is `~/mounted/workspace/myproject/.venv/bin/python3` pointing to, is the target of the symlink a "real" python or a pyenv shim? If it's the latter, recreating the venv with a python that's not a pyenv shim should help.

---

_Label `question` added by @zanieb on 2025-01-07 19:36_

---

_Comment by @ego-thales on 2025-01-13 13:01_

Sorry for delayed response, holidays... :)

> What is ~/mounted/workspace/myproject/.venv/bin/python3 pointing to

Pointing to `~/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3.13` so it seems ok I think?

---

_Label `question` removed by @konstin on 2025-01-13 19:32_

---

_Label `needs-mre` added by @konstin on 2025-01-13 19:32_

---

_Comment by @konstin on 2025-01-13 19:33_

The `uv -v run mypy` output is odd, i don't understand how pyenv is getting involved here.

---

_Comment by @ego-thales on 2025-01-15 10:06_

This is exactly what was startling me... But I think ultimately it has to do with filesystem corruption on my side.

I ended up creating a separate fresh new folder with uv init, and everything seems to work fine -- having set `UV_LINK_MODE` to `symlink` before.

---

_Closed by @ego-thales on 2025-01-15 10:06_

---

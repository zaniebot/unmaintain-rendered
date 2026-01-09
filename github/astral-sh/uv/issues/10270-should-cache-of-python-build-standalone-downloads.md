---
number: 10270
title: "Should cache of `python-build-standalone` downloads be invalidated?"
type: issue
state: open
author: tpgillam
labels:
  - needs-decision
assignees: []
created_at: 2025-01-02T10:07:34Z
updated_at: 2025-01-03T15:38:46Z
url: https://github.com/astral-sh/uv/issues/10270
synced_at: 2026-01-07T13:12:18-06:00
---

# Should cache of `python-build-standalone` downloads be invalidated?

---

_Issue opened by @tpgillam on 2025-01-02 10:07_

Context: I was wondering whether https://github.com/astral-sh/python-build-standalone/pull/421 might have helped with https://github.com/astral-sh/uv/issues/6893 (answer: 'no'). This required me knowing which build of python-build-standalone I was using.

Current `uv` behaviour seems to be that the version of `python-build-standalone` isn't included anywhere obvious, and we don't automatically invalidate our cache of downloaded interpreters based on this version.

e.g. we can update to a newer `uv` and still get an old python build:

```console
> uv --version
uv 0.5.13
> rm -r .venv/
> uv run python -VV
Using CPython 3.12.8
Creating virtual environment at: .venv
Installed 39 packages in 77ms
Python 3.12.8 (main, Dec  6 2024, 19:59:28) [Clang 18.1.8 ]
```

(Perhaps there's a better way (?) but the only way I could think to figure out what build of python-build-standalone we're using is by looking at the build time included in `python -VV`, so here December 6th 2024. )

From https://github.com/astral-sh/uv/issues/7036#issuecomment-2557272986, we need to explicitly remove or reinstall all downloaded versions to get the newer build:

```console
> uv python uninstall --all
Searching for Python installations
Uninstalled 4 versions in 181ms
 - cpython-3.11.9-linux-x86_64-gnu
 - cpython-3.12.5-linux-x86_64-gnu
 - cpython-3.12.8-linux-x86_64-gnu
 - cpython-3.13.0-linux-x86_64-gnu
> uv run python -VV
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
Using CPython 3.12.8
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 39 packages in 70ms
Python 3.12.8 (main, Dec 19 2024, 14:33:20) [Clang 18.1.8 ]
```
(NB python now build on December 19th)

Overall I find the existing behaviour a slightly opaque / unintuitive... but also it's a pretty niche problem! Nonetheless, maybe there's scope for a little improvement?

Some assorted ideas / thoughts:
- add documentation that we don't invalidate managed python downloads here: https://docs.astral.sh/uv/concepts/python-versions/#managed-python-distributions
- inclusion of python-build-standalone version in `uv python list` output
- sometimes (probably rarely!) useful to be able to specify the precise version of python-build-standalone?
- automatic switching of interpreter build version based on `uv` version... but this also feels a little unintuitive.
- if updating a managed python build, what should happen to existing venvs? I'm not too sure.


---

_Label `needs-decision` added by @charliermarsh on 2025-01-03 15:38_

---

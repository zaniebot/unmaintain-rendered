```yaml
number: 1785
title: "Use the right marker for the `implementation` field of `pyvenv.cfg`"
type: pull_request
state: merged
author: edgarrmondragon
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/pyvenv.cfg-implementation-field
created_at: 2024-02-20T22:04:09Z
updated_at: 2024-02-22T02:52:41Z
url: https://github.com/astral-sh/uv/pull/1785
synced_at: 2026-01-12T16:04:44Z
```

# Use the right marker for the `implementation` field of `pyvenv.cfg`

---

_@edgarrmondragon_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The generated `pyvenv.cfg` file hardcodes `implementation = CPython` even for PyPy venvs, created with `uv venv venv --python pypy3.10`, for example.

```ini
home = /path/to/.pyenv/versions/pypy3.10-7.3.15/bin
implementation = CPython
version_info = 3.10
gourgeist = 0.0.4
include-system-site-packages = false
base-prefix = /path/to/.pyenv/versions/pypy3.10-7.3.15
base-exec-prefix = /path/to/.pyenv/versions/pypy3.10-7.3.15
base-executable = /path/to/.pyenv/versions/pypy3.10-7.3.15/bin/pypy3.10
```

## Test Plan

<!-- How was it tested? -->

Manually verified that `pyvenv.cfg` now contains `implementation = PyPy`. I can try refactoring `create_bare_venv` to make it more easily testable, though.


---

_@charliermarsh approved on 2024-02-21 00:09_

Thanks!

---

_Merged by @charliermarsh on 2024-02-21 00:09_

---

_Closed by @charliermarsh on 2024-02-21 00:09_

---

_Label `bug` added by @charliermarsh on 2024-02-21 00:09_

---

_Branch deleted on 2024-02-21 00:24_

---

_Comment by @konstin on 2024-02-21 10:37_

@edgarrmondragon Is pypy otherwise working for you as it should? I'm asking because we haven't put much focus on pypy so far.

---

_Comment by @edgarrmondragon on 2024-02-22 02:48_

> @edgarrmondragon Is pypy otherwise working for you as it should? I'm asking because we haven't put much focus on pypy so far.

Hey @konstin! I'm testing things out myself. For starters `sys.path` seems to be broken in pypy venvs:

<details><summary>CPython 3.10</summary>
<p>

```console
$ pyenv shell 3.10
$ uv venv venv --python 3.10
$ source venv/bin/activate
$ (venv) python --version
Python 3.10.13
$ (venv) uv pip install requests
Resolved 5 packages in 502ms
Installed 5 packages in 18ms
 + certifi==2024.2.2
 + charset-normalizer==3.3.2
 + idna==3.6
 + requests==2.31.0
 + urllib3==2.2.1
$ (venv) python -c 'import requests; print(requests.get("https://google.com").status_code)'
200
$ (venv) python -c 'import sys; [print(f"{p=}") for p in sys.path]'
p=''
p='/Users/me/.pyenv/versions/3.10.13/lib/python310.zip'
p='/Users/me/.pyenv/versions/3.10.13/lib/python3.10'
p='/Users/me/.pyenv/versions/3.10.13/lib/python3.10/lib-dynload'
p='/Users/me/Code/repro/repro-uv-pypy-hatchling/venv/lib/python3.10/site-packages'
```

</p>
</details>

<details><summary>PyPy 3.10</summary>
<p>

```console
$ pyenv shell pypy3.10
$ uv venv venv --python pypy3.10
$ source venv/bin/activate
$ (venv) python --version
Python 3.10.13 (fc59e61cfbff, Jan 14 2024, 13:00:21)
[PyPy 7.3.15 with GCC Apple LLVM 13.1.6 (clang-1316.0.21.2.5)]
$ (venv) uv pip install requests
Resolved 5 packages in 10ms
Installed 5 packages in 6ms
 + certifi==2024.2.2
 + charset-normalizer==3.3.2
 + idna==3.6
 + requests==2.31.0
 + urllib3==2.2.1
$ (venv) python -c 'import requests; print(requests.get("https://google.com").status_code)'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'requests'
$ (venv) python -c 'import sys; [print(f"{p=}") for p in sys.path]'
p=''
p='/Users/me/.pyenv/versions/pypy3.10-7.3.15/lib/pypy3.10'
p='/Users/me/.pyenv/versions/pypy3.10-7.3.15/lib/pypy3.10/plat-mac'
p='/Users/me/.pyenv/versions/pypy3.10-7.3.15/lib/pypy3.10/plat-mac/lib-scriptpackages'
```

</p>
</details> 

Notice the venv's `/site-packages` is missing in the PyPy case.

Happy to open a new issue too!

---

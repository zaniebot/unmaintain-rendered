```yaml
number: 7154
title: "uv build --wheel doesn't seem to be installing dev-dependencies"
type: issue
state: closed
author: alex
labels:
  - bug
assignees: []
created_at: 2024-09-07T03:19:33Z
updated_at: 2024-09-07T14:32:26Z
url: https://github.com/astral-sh/uv/issues/7154
synced_at: 2026-01-12T15:59:11Z
```

# uv build --wheel doesn't seem to be installing dev-dependencies

---

_@alex_

https://github.com/pyca/cryptography/pull/11558 shows trying to take advantage of `uv build`, however CI doesn't seem happy: https://github.com/pyca/cryptography/actions/runs/10748158349/job/29811544879?pr=11558

Notably, when running `uv build -v --wheel --require-hashes --build-constraint=$BUILD_REQUIREMENTS_PATH cryptography*.tar.gz $PY_LIMITED_API -o wheelhouse/`:

```
DEBUG uv 0.4.7
DEBUG Searching for Python interpreter in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\runneradmin\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\hostedtoolcache\windows\Python\3.11.9\x64\python3.exe` (search path)
DEBUG Using request timeout of 30s
Building wheel from source distribution...
DEBUG Calling `maturin.build_wheel("D:\\a\\cryptography\\cryptography\\wheelhouse\\.tmpNob9dO", {"build-args":"--features=pyo3/abi3-py39"}, None)`
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'maturin'
error: Build backend failed to build wheel through `build_wheel()` with exit code: 1
```

For whatever reason, `uv` doesn't seem to actually install `maturin`. 

When running this same command locally (on macOS), I see output like the following:

```
(cryptography) ~/p/cryptography ❯❯❯ uv build -v --wheel --require-hashes --build-constraint=.github/requirements/build-requirements.txt dist/cryptography-44.0.0.dev1.tar.gz -o wheelhouse
DEBUG uv 0.4.6 (Homebrew 2024-09-05)
DEBUG Found workspace root: `/Users/alex_gaynor/projects/cryptography`
DEBUG Adding current workspace member: `/Users/alex_gaynor/projects/cryptography`
DEBUG Searching for Python >=3.7 in managed installations or system path
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/alex_gaynor/.virtualenvs/cryptography/bin/python3` (active virtual environment)
DEBUG Using request timeout of 30s
Building wheel from source distribution...
DEBUG Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: maturin>=1, <2
DEBUG Adding direct dependency: maturin==1.7.1
DEBUG Adding direct dependency: cffi{platform_python_implementation != 'PyPy'}>=1.12
DEBUG Adding direct dependency: cffi{platform_python_implementation != 'PyPy'}==1.17.1
DEBUG Adding direct dependency: setuptools<74.0.0 | >74.0.0, <74.1.0 | >74.1.0, <74.1.1 | >74.1.1, <74.1.2 | >74.1.2
DEBUG Adding direct dependency: setuptools==73.0.1
[...]
```

Happy to test anything that'd be useful.

---

_Label `bug` added by @charliermarsh on 2024-09-07 11:49_

---

_Comment by @charliermarsh on 2024-09-07 12:04_

Very strange, let me think about what could be happening here...

---

_Comment by @charliermarsh on 2024-09-07 12:05_

Do you have `--no-build-isolation` set anywhere, like in a `pyproject.toml` somewhere? (You could run `uv build --show-settings` to verify.)

---

_Comment by @charliermarsh on 2024-09-07 12:17_

(Poking around here: https://github.com/charliermarsh/cryptography/pull/1)

---

_Comment by @charliermarsh on 2024-09-07 12:21_

(Confirmed build isolation is _not_ disabled.)

---

_Comment by @alex on 2024-09-07 12:50_

Definitely not intentionally disabled! Thanks for taking a look, let me know how I can help debug this.

---

_Comment by @charliermarsh on 2024-09-07 12:51_

It looks like the builder _thinks_ build isolation is enabled (despite `--show-settings` not indicating as such) so something's going wrong...

---

_Comment by @alex on 2024-09-07 12:52_

AHHHHH, `--no-build-isolation` is in the `PY_LIMITED_API` variable. I assume that's causing this.

---

_Comment by @alex on 2024-09-07 12:55_

And with that removed, it seems to be working now.

So sorry about the noise.

I think turning some of the debug logging you added into proper logging may help future people who are as dumb as I am.

Once again, very sorry.

---

_Comment by @charliermarsh on 2024-09-07 12:57_

No problem at all! Glad it's not a bug at least. Though confused why `--show-settings` didn't make this clear... Maybe a bug _there_? I'll add some more logging for this regardless (and close this issue with that PR).

---

_Comment by @charliermarsh on 2024-09-07 13:02_

Oh sorry... Dumb of me. I'm running `--show-settings` in the first command which doesn't have the env var attached. I will add more logging though.

---

_Closed by @charliermarsh on 2024-09-07 14:32_

---

_Closed by @charliermarsh on 2024-09-07 14:32_

---

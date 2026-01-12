```yaml
number: 7164
title: "BUG/UX: confusing warning in `uvx` running `<tool>@latest` with `--resolution=lowest-direct`"
type: issue
state: open
author: neutrinoceros
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-09-07T11:25:16Z
updated_at: 2024-09-09T11:05:44Z
url: https://github.com/astral-sh/uv/issues/7164
synced_at: 2026-01-12T15:59:11Z
```

# BUG/UX: confusing warning in `uvx` running `<tool>@latest` with `--resolution=lowest-direct`

---

_@neutrinoceros_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

running `uvx` with `--resolution=lowest-direct` raises a warning that the tool itself is unpinned, even if I use version specifiers (`@latest` in the following example), and in turn can fail to install the tool. It looks like the version specifier is completely ignored, when it probably shouldn't ?

An example reproducer using pytest (I'm specifying the python version too in case it helps with reproduction)
```shell
$ uvx -p 3.12 --resolution=lowest-direct pytest@latest
```


output (abridged)
```
⠙ Resolving dependencies...
warning: The direct dependency `pytest` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pytest==2.0.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Downloading http://pypi.python.org/packages/source/d/distribute/distribute-0.6.14.tar.gz
Traceback (most recent call last):
  File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 143, in use_setuptools
    raise ImportError
ImportError

During handling of the above exception, another exception occurred:

Traceback (most recent call last):

(...)

urllib.error.HTTPError: HTTP Error 403: SSL is required
```

<details><summary>output (ful, including `--vebose`l)</summary>

```
DEBUG uv 0.4.6 (Homebrew 2024-09-05)
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/clm/Library/Application Support/uv/python`
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/clm/.pyenv/shims/python3` (search path)
DEBUG Using request timeout of 30s
DEBUG Caching via interpreter: `/Users/clm/.pyenv/versions/3.12.5/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: pytest*
warning: The direct dependency `pytest` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
DEBUG Found stale response for: https://pypi.org/simple/pytest/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest/
DEBUG Skipping prefetch for unbounded minimum-version range: pytest (*)
DEBUG Searching for a compatible version of pytest (*)
DEBUG Selecting: pytest==2.0.0 [compatible] (pytest-2.0.0.zip)
DEBUG Acquired lock for `/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b0/fc/5770d93a928ed76118eb21450307d065cbb6f359bdb11aebbef4e1ce93d3/pytest-2.0.0.zip
DEBUG No static `pyproject.toml` available for: pytest==2.0.0 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: pytest==2.0.0 (PkgInfo(UnsupportedMetadataVersion("1.0")))
DEBUG Found static `egg-info` for: pytest==2.0.0
DEBUG Released lock at `/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/.lock`
DEBUG Adding transitive dependency for pytest==2.0.0: py>=1.4.0a2
DEBUG Found stale response for: https://pypi.org/simple/py/
DEBUG Sending revalidation request for: https://pypi.org/simple/py/
DEBUG Found not-modified response for: https://pypi.org/simple/py/
DEBUG Searching for a compatible version of py (>=1.4.0a2)
DEBUG Selecting: py==1.11.0 [compatible] (py-1.11.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f6/f0/10642828a8dfb741e5f3fbaac830550a518a775c7fff6f04a007259b0548/py-1.11.0-py2.py3-none-any.whl.metadata
DEBUG Tried 2 versions: py 1, pytest 1
DEBUG Split specific environment resolution took 0.050s
Resolved 2 packages in 50ms
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: py
DEBUG Must revalidate requirement: pytest
DEBUG Acquired lock for `/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f6/f0/10642828a8dfb741e5f3fbaac830550a518a775c7fff6f04a007259b0548/py-1.11.0-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b0/fc/5770d93a928ed76118eb21450307d065cbb6f359bdb11aebbef4e1ce93d3/pytest-2.0.0.zip
DEBUG Building: pytest==2.0.0
DEBUG Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==74.1.2 [compatible] (setuptools-74.1.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.033s
DEBUG Installing in setuptools==74.1.2 in /Users/clm/.cache/uv/builds-v0/.tmpioDylQ
DEBUG Must revalidate requirement: setuptools
DEBUG Downloading and building requirement for build: setuptools==74.1.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl
DEBUG Installing build requirement: setuptools==74.1.2
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Downloading http://pypi.python.org/packages/source/d/distribute/distribute-0.6.14.tar.gz
DEBUG

DEBUG Traceback (most recent call last):
DEBUG

DEBUG   File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 143, in use_setuptools
DEBUG

DEBUG     raise ImportError
DEBUG

DEBUG ImportError
DEBUG

DEBUG
DEBUG

DEBUG During handling of the above exception, another exception occurred:
DEBUG

DEBUG
DEBUG

DEBUG Traceback (most recent call last):
DEBUG

DEBUG   File "<string>", line 14, in <module>
DEBUG

DEBUG   File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
DEBUG

DEBUG     return self._get_build_requires(config_settings, requirements=[])
DEBUG

DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
DEBUG

DEBUG     self.run_setup()
DEBUG

DEBUG   File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
DEBUG

DEBUG     super().run_setup(setup_script=setup_script)
DEBUG

DEBUG   File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
DEBUG

DEBUG     exec(code, locals())
DEBUG

DEBUG   File "<string>", line 4, in <module>
DEBUG

DEBUG   File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 145, in use_setuptools
DEBUG

DEBUG     return _do_download(version, download_base, to_dir, download_delay)
DEBUG

DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 123, in _do_download
DEBUG

DEBUG     tarball = download_setuptools(version, download_base,
DEBUG

DEBUG               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 193, in download_setuptools
DEBUG

DEBUG     src = urlopen(url)
DEBUG

DEBUG           ^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 215, in urlopen
DEBUG

DEBUG     return opener.open(url, data, timeout)
DEBUG

DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 521, in open
DEBUG

DEBUG     response = meth(req, response)
DEBUG

DEBUG                ^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 630, in http_response
DEBUG

DEBUG     response = self.parent.error(
DEBUG

DEBUG                ^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 559, in error
DEBUG

DEBUG     return self._call_chain(*args)
DEBUG

DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 492, in _call_chain
DEBUG

DEBUG     result = func(*args)
DEBUG

DEBUG              ^^^^^^^^^^^
DEBUG

DEBUG   File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 639, in http_error_default
DEBUG

DEBUG     raise HTTPError(req.full_url, code, msg, hdrs, fp)
DEBUG

DEBUG urllib.error.HTTPError: HTTP Error 403: SSL is required
DEBUG

DEBUG Released lock at `/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/.lock`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pytest==2.0.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Downloading http://pypi.python.org/packages/source/d/distribute/distribute-0.6.14.tar.gz
Traceback (most recent call last):
  File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 143, in use_setuptools
    raise ImportError
ImportError

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/clm/.cache/uv/builds-v0/.tmpioDylQ/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 4, in <module>
  File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 145, in use_setuptools
    return _do_download(version, download_base, to_dir, download_delay)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 123, in _do_download
    tarball = download_setuptools(version, download_base,
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.cache/uv/built-wheels-v3/pypi/pytest/2.0.0/ohiLnr2Yv5-L8iN34ez_v/pytest-2.0.0.zip/distribute_setup.py", line 193, in download_setuptools
    src = urlopen(url)
          ^^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 215, in urlopen
    return opener.open(url, data, timeout)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 521, in open
    response = meth(req, response)
               ^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 630, in http_response
    response = self.parent.error(
               ^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 559, in error
    return self._call_chain(*args)
           ^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 492, in _call_chain
    result = func(*args)
             ^^^^^^^^^^^
  File "/Users/clm/.pyenv/versions/3.12.5/lib/python3.12/urllib/request.py", line 639, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 403: SSL is required
---
```


</details>


```shell
$ uv --version
uv 0.4.6 (Homebrew 2024-09-05)
```

---

_Label `bug` added by @charliermarsh on 2024-09-07 12:00_

---

_Comment by @charliermarsh on 2024-09-07 12:01_

I think that's probably a bug, yeah, though it's a bit of a strange interaction... In some weird sense it's correct to use the lowest version there (it's the "most compatible" release).

---

_Comment by @neutrinoceros on 2024-09-07 12:10_

Maybe I should share my journey into finding this: I was trying to migrate my GHA workflows to astral-sh/setup-uv, and didn't want to special case Windows for env activation, so I tried not using `uv venv` at all, which is why I tried to convert this bit to `uvx`. I should also note that initially, I tried running this with `--with-environment req.txt` where my requirement file had an explicit lower bound for `pytest` (really, `coverage` in my case, though I don't think that's important). I removed that bit for the reproducer as the error is identical with or without it.

---

_Comment by @zanieb on 2024-09-07 15:57_

The nuance here is that `@latest` is just a short-hand to skip cached versions of the given package — we don't fetch the latest version for the package then resolve as if it was pinned to that. I'm not sure what we can do to resolve this yet, though I agree it's a bit of a rough interaction.

---

_Comment by @neutrinoceros on 2024-09-07 16:00_

I had the same issue with `@8.0.0`, is that expected too ?

---

_Comment by @zanieb on 2024-09-07 16:12_

No that's surprising.

---

_Comment by @neutrinoceros on 2024-09-07 17:37_

Nevermind, I'm taking that part back. Sorry, I should not be answering GitHub issues on my phone and from memory ! 😬

---

_Renamed from "BUG (?): crash in `uvx` running a specific version of a tool together with with `--resolution=lowest-direct`" to "BUG (?): crash in `uvx` running `<tool>@latest` with `--resolution=lowest-direct`" by @neutrinoceros on 2024-09-07 17:37_

---

_Comment by @neutrinoceros on 2024-09-07 17:38_

I edited the title to hopefully better capture the actual broken (or surprising) case.

---

_Comment by @neutrinoceros on 2024-09-09 10:00_

I realized that my report may have been confusing as it had a formatting issue and the warning I wanted to draw attention to wasn't immediately visible (edited). Here it is again
```
warning: The direct dependency `pytest` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
```
*this* is what I think is a bug in `uv`: it's confusing that it tells me I didn't pin pytest when I used `@latest`. I get now that the meaning of `@latest` isn't what I imagined at first, but this confusion could be avoided by clarifying it in the warning message and/or in docs. I think this would be sufficient to close this issue.


---

_Renamed from "BUG (?): crash in `uvx` running `<tool>@latest` with `--resolution=lowest-direct`" to "BUG/UX: confusing warning in `uvx` running `<tool>@latest` with `--resolution=lowest-direct`" by @neutrinoceros on 2024-09-09 10:06_

---

_Comment by @zanieb on 2024-09-09 10:59_

I sort of think we should opt a dependency out of the "lowest" resolution if `@latest` is used, but it may not be particularly simple and it seems like a rare case.

We could improve the message in the interim.

---

_Label `error messages` added by @zanieb on 2024-09-09 10:59_

---

_Comment by @neutrinoceros on 2024-09-09 11:05_

Yes, it would be the ideal solution for me too but it's maybe not worth the work or additional complexity. Anyway, improving the error message would go a long way for more cheaper !

---

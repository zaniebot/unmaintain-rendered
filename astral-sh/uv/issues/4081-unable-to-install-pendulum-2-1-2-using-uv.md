---
number: 4081
title: "Unable to install `pendulum==2.1.2` using `uv`"
type: issue
state: closed
author: mihirsamdarshi
labels:
  - question
assignees: []
created_at: 2024-06-05T23:59:19Z
updated_at: 2024-06-06T01:23:52Z
url: https://github.com/astral-sh/uv/issues/4081
synced_at: 2026-01-10T01:23:34Z
---

# Unable to install `pendulum==2.1.2` using `uv`

---

_Issue opened by @mihirsamdarshi on 2024-06-05 23:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv -v pip install pendulum==2.1.2` seems to error for me in a couple different environments.

I first noticed the error in my macOS `venv`. I was then able to reproduce it in a Docker container

The sequence of steps that reproduced the error for me on an Apple Silcon MacBook Pro were as follows. I also have attached the terminal output as well.

#### Reproducible Example

1. Create an interactive session in a basic Python 3.12 Docker container. 
 
```shell
docker run -it --rm python:3.12 /bin/bash
# error also occurs when emulating x86
docker run -it --rm --platform linux/amd64 python:3.12 /bin/bash
```

3. Install the latest version of `uv` using the official installer script
```shell
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.2.8/uv-installer.sh | sh
source ~/.cargo/env
```
_Note: This also occurs in version 0.2.5_

4. Create a `uv venv` and activate it

```shell
uv venv && source .venv/bin/activate
```

5. Fail to install `pendulum==2.1.2`

```shell
uv pip install pendulum==2.1.2
```

#### Output of Reproducible Example

```text
❯ docker run -it --rm python:3.12 /bin/bash
root@f6719cf5dd68:/# curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.2.8/uv-installer.sh | sh
downloading uv 0.2.8 aarch64-unknown-linux-gnu
installing to /root/.cargo/bin
  uv
everything's installed!

To add $HOME/.cargo/bin to your PATH, either restart your shell or run:

    source $HOME/.cargo/env (sh, bash, zsh)
    source $HOME/.cargo/env.fish (fish)

root@f6719cf5dd68:/# source ~/.cargo/env
root@f6719cf5dd68:/# uv venv
Using Python 3.12.3 interpreter at: usr/local/bin/python3
Creating virtualenv at: .venv
root@f6719cf5dd68:/# source .venv/bin/activate
(.venv) root@f6719cf5dd68:/# uv -v pip install pendulum==2.1.2
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: pendulum==2.1.2
DEBUG Using registry request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: pendulum==2.1.2
DEBUG No cache entry for: https://pypi.org/simple/pendulum/
DEBUG Searching for a compatible version of pendulum (==2.1.2)
DEBUG Selecting: pendulum==2.1.2 (pendulum-2.1.2.tar.gz)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/95/13/e7598039a7c2be1c74538125c035f866189897291f12da029a2a1008dc3e/pendulum-2.1.2-cp27-cp27m-macosx_10_15_x86_64.whl.metadata
DEBUG Adding transitive dependency for pendulum==2.1.2: python-dateutil>=2.6, <3.0
DEBUG Adding transitive dependency for pendulum==2.1.2: pytzdata>=2020.1
DEBUG No cache entry for: https://pypi.org/simple/python-dateutil/
DEBUG No cache entry for: https://pypi.org/simple/pytzdata/
DEBUG Searching for a compatible version of python-dateutil (>=2.6, <3.0)
DEBUG Selecting: python-dateutil==2.9.0.post0 (python_dateutil-2.9.0.post0-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl.metadata
DEBUG Adding transitive dependency for python-dateutil==2.9.0.post0: six>=1.5
DEBUG No cache entry for: https://pypi.org/simple/six/
DEBUG Searching for a compatible version of pytzdata (>=2020.1)
DEBUG Selecting: pytzdata==2020.1 (pytzdata-2020.1-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/4f/4474bda990ee740a020cbc3eb271925ef7daa7c8444240d34ff62c8442a3/pytzdata-2020.1-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of six (>=1.5)
DEBUG Selecting: six==1.16.0 (six-1.16.0-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl.metadata
DEBUG Tried 5 versions: pendulum 1, python-dateutil 1, pytzdata 1, root 1, six 1
Resolved 4 packages in 845ms
DEBUG Identified uncached requirement: pendulum==2.1.2
DEBUG Identified uncached requirement: python-dateutil==2.9.0.post0
DEBUG Identified uncached requirement: pytzdata==2020.1
DEBUG Identified uncached requirement: six==1.16.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/4f/4474bda990ee740a020cbc3eb271925ef7daa7c8444240d34ff62c8442a3/pytzdata-2020.1-py2.py3-none-any.whl
DEBUG Acquired lock for `/root/.cache/uv/built-wheels-v3/pypi/pendulum/2.1.2`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/db/15/6e89ae7cde7907118769ed3d2481566d05b5fd362724025198bb95faf599/pendulum-2.1.2.tar.gz
DEBUG Downloading source distribution: pendulum==2.1.2
DEBUG Building: pendulum==2.1.2
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: poetry-core>=1.0.0a9
DEBUG No cache entry for: https://pypi.org/simple/poetry-core/
DEBUG Searching for a compatible version of poetry-core (>=1.0.0a9)
DEBUG Selecting: poetry-core==1.9.0 (poetry_core-1.9.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a1/8d/85fcf9bcbfefcc53a1402450f28e5acf39dcfde3aabb996a1d98481ac829/poetry_core-1.9.0-py3-none-any.whl.metadata
DEBUG Tried 2 versions: poetry-core 1, root 1
DEBUG Installing in poetry-core==1.9.0 in /root/.cache/uv/.tmpIyOnb6/.venv
DEBUG Identified uncached requirement: poetry-core==1.9.0
DEBUG Downloading and building requirement for build: poetry-core==1.9.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a1/8d/85fcf9bcbfefcc53a1402450f28e5acf39dcfde3aabb996a1d98481ac829/poetry_core-1.9.0-py3-none-any.whl
DEBUG Installing build requirement: poetry-core==1.9.0
DEBUG Calling `poetry.core.masonry.api.get_requires_for_build_wheel()`
DEBUG Calling `poetry.core.masonry.api.build_wheel("/root/.cache/uv/built-wheels-v3/pypi/pendulum/2.1.2/ByoYWHVpQUGO2IqO_ZlPb/.tmplVuW65", {}, None)`
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pendulum==2.1.2
  Caused by: Failed to build: `pendulum==2.1.2`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "/root/.cache/uv/built-wheels-v3/pypi/pendulum/2.1.2/ByoYWHVpQUGO2IqO_ZlPb/pendulum-2.1.2.tar.gz/build.py", line 5, in <module>
    from distutils.command.build_ext import build_ext
ModuleNotFoundError: No module named 'distutils'
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/root/.cache/uv/.tmpIyOnb6/.venv/lib/python3.12/site-packages/poetry/core/masonry/api.py", line 58, in build_wheel
    return WheelBuilder.make_in(
           ^^^^^^^^^^^^^^^^^^^^^
  File "/root/.cache/uv/.tmpIyOnb6/.venv/lib/python3.12/site-packages/poetry/core/masonry/builders/wheel.py", line 88, in make_in
    wb.build(target_dir=directory)
  File "/root/.cache/uv/.tmpIyOnb6/.venv/lib/python3.12/site-packages/poetry/core/masonry/builders/wheel.py", line 123, in build
    self._build(zip_file)
  File "/root/.cache/uv/.tmpIyOnb6/.venv/lib/python3.12/site-packages/poetry/core/masonry/builders/wheel.py", line 172, in _build
    self._run_build_script(self._package.build_script)
  File "/root/.cache/uv/.tmpIyOnb6/.venv/lib/python3.12/site-packages/poetry/core/masonry/builders/wheel.py", line 262, in _run_build_script
    subprocess.check_call([self.executable.as_posix(), build_script])
  File "/usr/local/lib/python3.12/subprocess.py", line 413, in check_call
    raise CalledProcessError(retcode, cmd)
subprocess.CalledProcessError: Command '['/root/.cache/uv/.tmpIyOnb6/.venv/bin/python', 'build.py']' returned non-zero exit status 1.
---
(.venv) root@f6719cf5dd68:/#
```

#### `uv` Info

- **macOS**: `uv 0.2.5`, `arm64`
- **Linux**: `uv 0.2.8`, `arm64`


---

_Comment by @zanieb on 2024-06-06 00:20_

Hi @mihirsamdarshi 

I believe the problem is that [`distutils` was removed in Python 3.12](https://docs.python.org/3.10/library/distutils.html) so the Pendulum build script fails to import it.

---

_Label `question` added by @zanieb on 2024-06-06 00:20_

---

_Comment by @charliermarsh on 2024-06-06 00:55_

👍 I don't think pip is able to install this either. It's an issue with the `pendulum` package itself.

---

_Closed by @charliermarsh on 2024-06-06 00:55_

---

_Comment by @mihirsamdarshi on 2024-06-06 01:03_

Thanks, my bad, I should've check that before posting this issue

---

_Comment by @charliermarsh on 2024-06-06 01:23_

No prob.

---

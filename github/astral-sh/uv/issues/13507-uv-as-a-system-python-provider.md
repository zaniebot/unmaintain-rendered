---
number: 13507
title: "`uv` as a system Python provider"
type: issue
state: closed
author: Andrej730
labels:
  - question
assignees: []
created_at: 2025-05-17T12:52:46Z
updated_at: 2025-05-31T11:25:54Z
url: https://github.com/astral-sh/uv/issues/13507
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv` as a system Python provider

---

_Issue opened by @Andrej730 on 2025-05-17 12:52_

### Question

Is it possible to use `uv` to provide globally available (*system*?) python? Or it's out of scope for `uv` (e.g. `uv` is focused on project package management *only*) and I should install Python from elsewhere?

I mean I can use  `uv python install 3.11 --preview` to make it globally available as `python3.11` (or even `--default` for `python`), but it's not very useful as global Python as `uv pip install` works only for venvs.

To understand my pov - I wanted to try to switch to `uv` completely, but most of the scripts on the system are using simple `python xxx.py` to run, so they do need system installed dependencies and since `uv pip install xxx --system` won't work on `uv python install` installations as they're *managed*. And it seems the only way to make it work is to have two Python instances of the same version installed (one from `uv`, one from elsewhere).

If it would be possible, it would help users transition to `uv` more seamlessly.

### Platform

Windows 11

### Version

uv 0.7.5 (9d1a14e1f 2025-05-16)

---

_Label `question` added by @Andrej730 on 2025-05-17 12:52_

---

_Comment by @eegli on 2025-05-18 15:27_

If you really need a "system-wide" like environment and cannot use inline dependencies in scripts, is there anything that goes against creating a dedicated virtual environment somewhere and then using this globally? One could activate the virtual env on shell startup and make the "system" python available in the path.

```sh
# initial setup, needs to be run once
mkdir uv-global && cd uv-global
uv venv --python 3.13

# adjust shell and paths for Windows if needed
echo "export PATH=\"$(realpath .venv/bin):\$PATH\"" >> ~/.bashrc
echo "source \"$(realpath .venv/bin/activate)\"" >> ~/.bashrc

# initialize
source ~/.bashrc
```

For managing packages, you'll have to cd to `uv-global` and do `uv pip install xyz`:

```sh
cd uv-global
uv pip install polars
```

Finally, your new global env should be good to go:

`python -c "import polars; print(polars.__version__)"`



---

_Comment by @konstin on 2025-05-19 07:54_

For your use case, there is inline script metadata and `uv run script.py`, which will install the dependencies with your script. We intentionally avoid having a single big global environment with packages for multiple scripts and instead support per-script isolated environments and tool installations.

---

_Comment by @Andrej730 on 2025-05-19 19:32_

> For your use case, there is inline script metadata and `uv run script.py`, which will install the dependencies with your script. We intentionally avoid having a single big global environment with packages for multiple scripts and instead support per-script isolated environments and tool installations.

I understand, but the transition to this workflow is hard and sometimes is not very reasonable.

> If you really need a "system-wide" like environment and cannot use inline dependencies in scripts, is there anything that goes against creating a dedicated virtual environment somewhere and then using this globally? One could activate the virtual env on shell startup and make the "system" python available in the path.

That's an interesting approach, it's cleaner then `uv pip install xxxx --system --break-system-packages` I've currently taken for global dependencies. 

Not sure how reliable this would be on Unix, but on Windows it's very hard to convince system to always activate venv before anything it does. So I've taken a different approach -  I've created "system venv", then added 'systemvenv.pth' file with contents 'L:\Software\uv\systemvenv\.venv\Lib\site-packages' to my system python's folder 'cpython-3.11.12-windows-x86_64-none\Lib\site-packages. At first I thought about referencing this venv site-packages in `PYTHONPATH`, but that may conflict with other Python instances at some point,

And I've also defined `pip.bat` as a shim for using `pip`:
```bat
uv pip %* --directory L:\Software\uv\systemvenv
```
So I can use `pip install xxx` / `pip uninstall xxx` to access "system" environment.

The one caveat I've currently met that I cannot use `pip install -r pyproject.toml` anymore, since `--directory L:\Software\uv\systemvenv` basically does `cd L:\Software\uv\systemvenv` before command execution, so it's looking for 'pyproject.toml' in 'L:\Software\uv\systemvenv'. The workaround is to provide absolute path `pip install -r L:\xxx\yyy\pyproject.toml`.

I've tried to use `--project` like below, but I guess because of the presense of pyproject.toml it starts to looking for venv in the current directory, so `--directory` is the best option for now for this shim.
```bat
:: error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
uv pip install -r pyproject.toml --project L:\Software\uv\systemvenv
```


Update. A note for anyone stumbling upon this, `.pth` can significantly slowdown Python startup time and adding `L:\Software\uv\systemvenv\.venv\Scripts` to PATH turned to to work much better.



---

_Comment by @charliermarsh on 2025-05-20 13:28_

Yeah, it is intentional that we don't support using these as system Pythons.

---

_Closed by @charliermarsh on 2025-05-20 13:28_

---

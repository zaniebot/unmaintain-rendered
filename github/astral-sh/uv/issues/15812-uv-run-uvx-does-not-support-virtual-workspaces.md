---
number: 15812
title: uv run / uvx does not support virtual workspaces
type: issue
state: open
author: seandlg
labels:
  - bug
assignees: []
created_at: 2025-09-12T14:12:47Z
updated_at: 2025-09-12T16:02:19Z
url: https://github.com/astral-sh/uv/issues/15812
synced_at: 2026-01-07T13:12:19-06:00
---

# uv run / uvx does not support virtual workspaces

---

_Issue opened by @seandlg on 2025-09-12 14:12_

### Summary

When building a virtual workspace locally

```bash
git clone https://github.com/astral-sh/uv.git
cd uv/scripts/workspaces/albatross-virtual-workspace
uv build
```

this works, with warnings:

```bash
uv build
Building source distribution...
warning: `/Users/seandlg/Coding/Playground/uv/scripts/workspaces/albatross-virtual-workspace` appears to be a workspace root without a Python project; consider using `uv sync` to install the workspace, or add a `[build-system]` table to `pyproject.toml`
No parent package detected, impossible to derive `name`
running egg_info
writing UNKNOWN.egg-info/PKG-INFO
writing dependency_links to UNKNOWN.egg-info/dependency_links.txt
writing top-level names to UNKNOWN.egg-info/top_level.txt
reading manifest file 'UNKNOWN.egg-info/SOURCES.txt'
writing manifest file 'UNKNOWN.egg-info/SOURCES.txt'
No parent package detected, impossible to derive `name`
running sdist
running egg_info
writing UNKNOWN.egg-info/PKG-INFO
writing dependency_links to UNKNOWN.egg-info/dependency_links.txt
writing top-level names to UNKNOWN.egg-info/top_level.txt
reading manifest file 'UNKNOWN.egg-info/SOURCES.txt'
writing manifest file 'UNKNOWN.egg-info/SOURCES.txt'
warning: sdist: standard file not found: should have one of README, README.rst, README.txt, README.md

running check
warning: check: missing required meta-data: name

creating unknown-0.0.0
creating unknown-0.0.0/UNKNOWN.egg-info
creating unknown-0.0.0/packages/albatross
creating unknown-0.0.0/packages/albatross/src/albatross
creating unknown-0.0.0/packages/seeds/src/seeds
copying files to unknown-0.0.0...
copying pyproject.toml -> unknown-0.0.0
copying UNKNOWN.egg-info/PKG-INFO -> unknown-0.0.0/UNKNOWN.egg-info
copying UNKNOWN.egg-info/SOURCES.txt -> unknown-0.0.0/UNKNOWN.egg-info
copying UNKNOWN.egg-info/dependency_links.txt -> unknown-0.0.0/UNKNOWN.egg-info
copying UNKNOWN.egg-info/top_level.txt -> unknown-0.0.0/UNKNOWN.egg-info
copying packages/albatross/check_installed_albatross.py -> unknown-0.0.0/packages/albatross
copying packages/albatross/src/albatross/__init__.py -> unknown-0.0.0/packages/albatross/src/albatross
copying packages/seeds/src/seeds/__init__.py -> unknown-0.0.0/packages/seeds/src/seeds
copying UNKNOWN.egg-info/SOURCES.txt -> unknown-0.0.0/UNKNOWN.egg-info
Writing unknown-0.0.0/setup.cfg
Creating tar archive
removing 'unknown-0.0.0' (and everything under it)
Building wheel from source distribution...
No parent package detected, impossible to derive `name`
running egg_info
writing UNKNOWN.egg-info/PKG-INFO
writing dependency_links to UNKNOWN.egg-info/dependency_links.txt
writing top-level names to UNKNOWN.egg-info/top_level.txt
reading manifest file 'UNKNOWN.egg-info/SOURCES.txt'
writing manifest file 'UNKNOWN.egg-info/SOURCES.txt'
No parent package detected, impossible to derive `name`
running bdist_wheel
running build
running build_py
creating build/lib/packages/albatross
copying packages/albatross/check_installed_albatross.py -> build/lib/packages/albatross
creating build/lib/packages/seeds/src/seeds
copying packages/seeds/src/seeds/__init__.py -> build/lib/packages/seeds/src/seeds
creating build/lib/packages/albatross/src/albatross
copying packages/albatross/src/albatross/__init__.py -> build/lib/packages/albatross/src/albatross
installing to build/bdist.macosx-11.0-arm64/wheel
running install
running install_lib
creating build/bdist.macosx-11.0-arm64/wheel
creating build/bdist.macosx-11.0-arm64/wheel/packages
creating build/bdist.macosx-11.0-arm64/wheel/packages/seeds
creating build/bdist.macosx-11.0-arm64/wheel/packages/seeds/src
creating build/bdist.macosx-11.0-arm64/wheel/packages/seeds/src/seeds
copying build/lib/packages/seeds/src/seeds/__init__.py -> build/bdist.macosx-11.0-arm64/wheel/./packages/seeds/src/seeds
creating build/bdist.macosx-11.0-arm64/wheel/packages/albatross
copying build/lib/packages/albatross/check_installed_albatross.py -> build/bdist.macosx-11.0-arm64/wheel/./packages/albatross
creating build/bdist.macosx-11.0-arm64/wheel/packages/albatross/src
creating build/bdist.macosx-11.0-arm64/wheel/packages/albatross/src/albatross
copying build/lib/packages/albatross/src/albatross/__init__.py -> build/bdist.macosx-11.0-arm64/wheel/./packages/albatross/src/albatross
running install_egg_info
running egg_info
writing UNKNOWN.egg-info/PKG-INFO
writing dependency_links to UNKNOWN.egg-info/dependency_links.txt
writing top-level names to UNKNOWN.egg-info/top_level.txt
reading manifest file 'UNKNOWN.egg-info/SOURCES.txt'
writing manifest file 'UNKNOWN.egg-info/SOURCES.txt'
Copying UNKNOWN.egg-info to build/bdist.macosx-11.0-arm64/wheel/./UNKNOWN-0.0.0-py3.14.egg-info
running install_scripts
creating build/bdist.macosx-11.0-arm64/wheel/unknown-0.0.0.dist-info/WHEEL
creating '/Users/seandlg/Coding/Playground/uv/scripts/workspaces/albatross-virtual-workspace/dist/.tmp-k2kn3bdk/unknown-0.0.0-py3-none-any.whl' and adding 'build/bdist.macosx-11.0-arm64/wheel' to it
adding 'packages/albatross/check_installed_albatross.py'
adding 'packages/albatross/src/albatross/__init__.py'
adding 'packages/seeds/src/seeds/__init__.py'
adding 'unknown-0.0.0.dist-info/METADATA'
adding 'unknown-0.0.0.dist-info/WHEEL'
adding 'unknown-0.0.0.dist-info/top_level.txt'
adding 'unknown-0.0.0.dist-info/RECORD'
removing build/bdist.macosx-11.0-arm64/wheel
Successfully built dist/unknown-0.0.0.tar.gz
Successfully built dist/unknown-0.0.0-py3-none-any.whl
```

However, when attempting to load this workspace with `uv run` I observe the following error:

```bash
uv run --isolated --with 'git+https://github.com/astral-sh/uv.git@main#subdirectory=scripts/workspaces/albatross-virtual-workspace' python3
      Built albatross @ file:///Users/seandlg/Coding/Playground/uv/scripts/workspaces/albatross-virtual-workspace/packages/albatross
      Built bird-feeder @ file:///Users/seandlg/Coding/Playground/uv/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder
      Built seeds @ file:///Users/seandlg/Coding/Playground/uv/scripts/workspaces/albatross-virtual-workspace/packages/seeds
Installed 7 packages in 9ms
⠙ Resolving dependencies...
warning: `/Users/seandlg/.cache/uv/git-v0/checkouts/5d8dbe36cd6ea14f/8f3583a6e/scripts/workspaces/albatross-virtual-workspace` appears to be a workspace root without a Python project; consider using `uv sync` to install the workspace, or add a `[build-system]` table to `pyproject.toml`
  × Failed to resolve `--with` requirement
  ╰─▶ Failed to parse metadata from built wheel
```

and with `uvx`:

```bash
uvx --isolated --from 'git+https://github.com/astral-sh/uv.git@main#subdirectory=scripts/workspaces/albatross-virtual-workspace' python3
⠹ Resolving dependencies...
warning: `/Users/seandlg/.cache/uv/git-v0/checkouts/5d8dbe36cd6ea14f/8f3583a6e/scripts/workspaces/albatross-virtual-workspace` appears to be a workspace root without a Python project; consider using `uv sync` to install the workspace, or add a `[build-system]` table to `pyproject.toml`
  × Failed to resolve `--with` requirement
  ╰─▶ Failed to parse metadata from built wheel
```

I have omitted verbose logs, as these are easy to produce locally. It would be great if `uv` supported loading packages from virtual workspaces.

For reference, this works flawlessly in non-virtual workspaces:

```bash
uv run --isolated --with 'git+https://github.com/astral-sh/uv.git@main#subdirectory=scripts/workspaces/albatross-root-workspace' python3 -c 'from albatross import fly; fly(); print("Success!")'
Installed 7 packages in 16ms
      Built seeds @ git+https://github.com/astral-sh/uv.git@8f3583a6e63800b770fec3b7be9b754be9d65602#subdirectory=scripts/workspaces/albatross-root-workspace/packag
      Built bird-feeder @ git+https://github.com/astral-sh/uv.git@8f3583a6e63800b770fec3b7be9b754be9d65602#subdirectory=scripts/workspaces/albatross-root-workspace/
      Built albatross @ git+https://github.com/astral-sh/uv.git@8f3583a6e63800b770fec3b7be9b754be9d65602#subdirectory=scripts/workspaces/albatross-root-workspace
Installed 5 packages in 3ms
Success!
```

and

```bash
uvx --isolated --from 'git+https://github.com/astral-sh/uv.git@main#subdirectory=scripts/workspaces/albatross-root-workspace' python3 -c 'from albatross import fly; fly(); print("Success!")'
Success!
```

A side note: It's great that `uv` includes [reference workspaces in this repository](https://github.com/astral-sh/uv/tree/main/scripts/workspaces); I think these should be linked in the public documentation. Happy to provide an MR, if you agree. 

### Platform

macOS 15.6.1

### Version

uv 0.8.17 (Homebrew 2025-09-10)

### Python version

3.13.7

---

_Label `bug` added by @seandlg on 2025-09-12 14:12_

---

_Comment by @charliermarsh on 2025-09-12 14:54_

Thanks for the clear write-up. Can you give an example of what you're trying to do with `--with` pointing to a virtual workspace? Whatever you target with `--with` has to be built and installed as a package (e.g., it's generally for things that you'd then `import`), but virtual workspaces by definition aren't meant to be built as packages.

---

_Comment by @seandlg on 2025-09-12 16:01_

Thanks for the quick reply, and sure! I'm playing with a workflow for debugging parts of a virtual workspace. Constructing an example for [albatross-virtual-workspace](https://github.com/astral-sh/uv/tree/main/scripts/workspaces/albatross-virtual-workspace): If package `seeds` had `src/client.py`, s.t.:

```py
import binascii

import httpx


class SeedClient:
    def __init__(self, base_url: str = "https://httpbingo.org/"):
        self.client = httpx.Client(base_url=base_url)

    def get_seed(self, seed_id: str):
        response = self.client.get(f"/bytes/{seed_id}")
        return binascii.hexlify(response.content).decode()
```

I'd like to use/debug this client using:

```bash
uv run --isolated  --with ipython --with 'git+https://github.com/astral-sh/uv.git@main#subdirectory=scripts/workspaces/albatross-virtual-workspace' ipython
```

To be able to do things like:

```python
# assume import
In [1]: c = SeedClient()

In [2]: c.get_seed(5)
Out[2]: '14df57fa82'
```

---

A nice solution could be to allow for pulling only select packages into scope. This does not currently seem to work though, neither for virtual, nor normal workspaces.

```bash
uv run --isolated --package albatross --with 'git+https://github.com/astral-sh/uv.git#subdirectory=scripts/workspaces/albatross-root-workspace' python3
error: Package `albatross` not found in workspace
```

I suppose that `--package` and `--all-packages` are evaluated before the `--with` overlays are applied, because otherwise it's unclear which `--with` overlay to resolve the `--package` from…

---

---
number: 12856
title: "`uv lock` and `uv sync` will contaminate other conda environments when installing an editable package locally"
type: issue
state: closed
author: zhuoqun-chen
labels:
  - needs-mre
assignees: []
created_at: 2025-04-14T01:54:58Z
updated_at: 2025-04-14T16:19:37Z
url: https://github.com/astral-sh/uv/issues/12856
synced_at: 2026-01-10T01:25:26Z
---

# `uv lock` and `uv sync` will contaminate other conda environments when installing an editable package locally

---

_Issue opened by @zhuoqun-chen on 2025-04-14 01:54_

### Summary

I found that if I'm already inside a conda env, then using `uv` project level APIs to install an editable package will accidentally make other conda env detect this package as installed in their isolated environment and cannot be uninstalled.

The hack for letting uv to install the package as editable package is borrowed from: https://github.com/astral-sh/uv/issues/1626#issuecomment-2394365983


Here are the steps to reproduce:

```shell
mkdir -p ~/work/foo-project
cd ~/work/foo-project
conda create -n foo-project python=3.10
conda activate foo-project

which pip
# /home/zqchen/miniconda3/envs/foo-project/bin/pip

which uv
# /home/linuxbrew/.linuxbrew/bin/uv

uv --version
# uv 0.6.14 (Homebrew 2025-04-09)

# 1.
uv init
# And edit `pyproject.toml` with additional lines to make foo-project installed as an editable package:
# see also https://github.com/astral-sh/uv/issues/1626#issuecomment-2394365983

# [build-system]
# requires = ["setuptools"]
# build-backend = "setuptools.build_meta"

# 2.
uv venv
# Using CPython 3.10.17 interpreter at: /home/zqchen/miniconda3/envs/foo-project/bin/python3.10
# Creating virtual environment at: .venv
# Activate with: source .venv/bin/activate

# 3.
uv sync --verbose
```

```log
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/home/zqchen/work/foo-project`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/zqchen/work/foo-project`
DEBUG Reading Python requests from version file at `/home/zqchen/work/foo-project/.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.10`
DEBUG Released lock at `/tmp/uv-cd6b5794ebe73e42.lock`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to mismatched source: `foo-project` (expected: `editable`)
DEBUG Found static `pyproject.toml` for: foo-project @ file:///home/zqchen/work/foo-project
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10.17
DEBUG Solving with target Python version: >=3.10
DEBUG Adding direct dependency: foo-project*
DEBUG Searching for a compatible version of foo-project @ file:///home/zqchen/work/foo-project (*)
DEBUG Tried 1 versions: foo-project 1
DEBUG all marker environments resolution took 0.000s
Resolved 1 package in 7ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: foo-project @ file:///home/zqchen/work/foo-project
DEBUG Acquired lock for `/home/zqchen/.cache/uv/sdists-v9/editable/e32f074a57be94bb`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1744594801, tv_nsec: 671946632 })), None, None, {}, {"src": None}
   Building foo-project @ file:///home/zqchen/work/foo-project
DEBUG Building: foo-project @ file:///home/zqchen/work/foo-project
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /home/zqchen/work/foo-project/.venv/bin/python3
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.10.17
DEBUG Solving with target Python version: >=3.10.17
DEBUG Adding direct dependency: setuptools*
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==78.1.0 [compatible] (setuptools-78.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/21/f43f0a1fa8b06b32812e0975981f4677d28e0f3271601dc88ac5a5b83220/setuptools-78.1.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.048s
DEBUG Installing in setuptools==78.1.0 in /home/zqchen/.cache/uv/builds-v0/.tmpYmLQE1
DEBUG Registry requirement already cached: setuptools==78.1.0
DEBUG Installing build requirement: setuptools==78.1.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_editable()`
DEBUG running egg_info
DEBUG creating foo_project.egg-info
DEBUG writing foo_project.egg-info/PKG-INFO
DEBUG writing dependency_links to foo_project.egg-info/dependency_links.txt
DEBUG writing top-level names to foo_project.egg-info/top_level.txt
DEBUG writing manifest file 'foo_project.egg-info/SOURCES.txt'
DEBUG reading manifest file 'foo_project.egg-info/SOURCES.txt'
DEBUG writing manifest file 'foo_project.egg-info/SOURCES.txt'
DEBUG No workspace root found, using project root
DEBUG Calling `setuptools.build_meta.build_editable("/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN", {}, None)`
DEBUG running editable_wheel
DEBUG creating /home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info
DEBUG writing /home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/PKG-INFO
DEBUG writing dependency_links to /home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/dependency_links.txt
DEBUG writing top-level names to /home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/top_level.txt
DEBUG writing manifest file '/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/SOURCES.txt'
DEBUG reading manifest file '/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/SOURCES.txt'
DEBUG writing manifest file '/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project.egg-info/SOURCES.txt'
DEBUG creating '/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project-0.1.0.dist-info'
DEBUG creating /home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project-0.1.0.dist-info/WHEEL
DEBUG running build_py
DEBUG running egg_info
DEBUG creating /tmp/tmpsv6231hu.build-temp/foo_project.egg-info
DEBUG writing /tmp/tmpsv6231hu.build-temp/foo_project.egg-info/PKG-INFO
DEBUG writing dependency_links to /tmp/tmpsv6231hu.build-temp/foo_project.egg-info/dependency_links.txt
DEBUG writing top-level names to /tmp/tmpsv6231hu.build-temp/foo_project.egg-info/top_level.txt
DEBUG writing manifest file '/tmp/tmpsv6231hu.build-temp/foo_project.egg-info/SOURCES.txt'
DEBUG reading manifest file '/tmp/tmpsv6231hu.build-temp/foo_project.egg-info/SOURCES.txt'
DEBUG writing manifest file '/tmp/tmpsv6231hu.build-temp/foo_project.egg-info/SOURCES.txt'
DEBUG Editable install will be performed using a meta path finder.
DEBUG 
DEBUG Options like `package-data`, `include/exclude-package-data` or
DEBUG `packages.find.exclude/include` may have no effect.
DEBUG 
DEBUG adding '__editable___foo_project_0_1_0_finder.py'
DEBUG adding '__editable__.foo_project-0.1.0.pth'
DEBUG creating '/home/zqchen/.cache/uv/builds-v0/.tmpy4KfeN/.tmp-lnp2nh93/foo_project-0.1.0-0.editable-py3-none-any.whl' and adding '/tmp/tmp35_zmv2rfoo_project-0.1.0-0.editable-py3-none-any.whl' to it
DEBUG adding 'foo_project-0.1.0.dist-info/METADATA'
DEBUG adding 'foo_project-0.1.0.dist-info/WHEEL'
DEBUG adding 'foo_project-0.1.0.dist-info/top_level.txt'
DEBUG adding 'foo_project-0.1.0.dist-info/RECORD'
DEBUG /home/zqchen/.cache/uv/builds-v0/.tmpYmLQE1/lib/python3.10/site-packages/setuptools/command/editable_wheel.py:342: InformationOnly: Editable installation.
DEBUG !!
DEBUG 
DEBUG         ********************************************************************************
DEBUG         Please be careful with folders in your working directory with the same
DEBUG         name as your package as they may take precedence during imports.
DEBUG         ********************************************************************************
DEBUG 
DEBUG !!
DEBUG   with strategy, WheelFile(wheel_path, "w") as wheel_obj:
DEBUG Finished building: foo-project @ file:///home/zqchen/work/foo-project
      Built foo-project @ file:///home/zqchen/work/foo-project
DEBUG Released lock at `/home/zqchen/.cache/uv/sdists-v9/editable/e32f074a57be94bb/.lock`
Prepared 1 package in 407ms
Installed 1 package in 0.35ms
 + foo-project==0.1.0 (from file:///home/zqchen/work/foo-project)
```

The unexpected behavior is shown below. Even though `conda base` shouldn't know the existence of `foo-project` at all, yet still it detected this package and there is no way to remove it. This is true for other `conda envs`, they are all aware of this package.

```shell
# 4.
conda activate base
which pip
# /home/zqchen/miniconda3/bin/pip

pip list | grep "foo-project"
# foo-project                   0.1.0

# 5.
pip uninstall foo-project
# Found existing installation: foo-project 0.1.0
# Can't uninstall 'foo-project'. No files were found to uninstall.

# 6.
conda remove foo-project                              

# PackagesNotFoundError: The following packages are missing from the target environment:
#   - foo-project
```

### Platform

Linux 5.15.0-136-generic x86_64 GNU/Linux

### Version

uv 0.6.14 (Homebrew 2025-04-09)

### Python version

Python 3.10.17

---

_Label `bug` added by @zhuoqun-chen on 2025-04-14 01:54_

---

_Renamed from "`uv lock` and `uv sync` will contaminate other conda environments if when installing an editable package locally" to "`uv lock` and `uv sync` will contaminate other conda environments when installing an editable package locally" by @zhuoqun-chen on 2025-04-14 01:55_

---

_Comment by @konstin on 2025-04-14 08:44_

fwiw you use `uv init --lib` to add a build system directly.

Unfortunately, I can't reproduce this: Running the commands you shared, `pip list` does not show foo-project. foo-project is only installed in `.venv` for me and only `uv pip list` after activating `.venv` shows it for me.

---

_Label `needs-mre` added by @konstin on 2025-04-14 08:44_

---

_Label `bug` removed by @konstin on 2025-04-14 08:44_

---

_Comment by @zhuoqun-chen on 2025-04-14 16:04_

Hi @konstin, thank you for the quick reply. After I restarted my computer, without doing anything besides above, I can't reproduce the conda env specific `pip list` behavior either, it didn't show `foo-project` is installed, which is good. My guess is `uv` installed some files in `/tmp` or `${HOME}/.cache` and they are wrongly recognized by `conda`.

And thanks for your `uv init --lib` option hint, it works like a charm! `uv` really is a great software!

---

_Closed by @zhuoqun-chen on 2025-04-14 16:04_

---

_Comment by @zhuoqun-chen on 2025-04-14 16:19_

Oh wait, I found that the strange behavior only shows up in `vscode` terminals, not regular `ssh session` I launched separately.

---

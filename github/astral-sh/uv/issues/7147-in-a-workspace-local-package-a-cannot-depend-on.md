---
number: 7147
title: in a workspace local package A cannot depend on local package B as a build-system requirement
type: issue
state: closed
author: mmerickel
labels:
  - bug
assignees: []
created_at: 2024-09-06T22:56:03Z
updated_at: 2024-10-16T00:46:56Z
url: https://github.com/astral-sh/uv/issues/7147
synced_at: 2026-01-07T13:12:17-06:00
---

# in a workspace local package A cannot depend on local package B as a build-system requirement

---

_Issue opened by @mmerickel on 2024-09-06 22:56_

This is the same issue as on https://github.com/astral-sh/rye/issues/813 but I'm reproducing it here using uv's new workspace features to replace rye with uv.

## Repro Structure

### pyproject.toml
```toml
[project]
name = "my-workspace"
version = "0.0.0"
dependencies = [
    "my-app",
]

[tool.uv]
package = false

[tool.uv.sources]
my-app = { workspace = true }
my-setuptools-extension = { workspace = true }

[tool.uv.workspace]
members = [
    "src/my-setuptools-extension",
    "src/my-app",
]
```

### src/my-setuptools-extension/pyproject.toml
```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
]
build-backend = "setuptools.build_meta"

[project]
name = "my-setuptools-extension"
version = "0.0.0"

dependencies = [
    "setuptools",
]

[project.entry-points."distutils.commands"]
build_webassets = "my_setuptools_ext:BuildWebAssetsCommand"

[tool.setuptools]
py-modules = ["my_setuptools_ext"]
```

### src/my-setuptools-extension/my_setuptools_ext.py
```python
from setuptools import Command


class BuildWebAssetsCommand(Command):
    def initialize_options(self):
        pass

    def finalize_options(self):
        pass

    def run(self):
        print('running!!!')
```

### src/my-app/pyproject.toml
```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
    "my-setuptools-extension",
]
build-backend = "setuptools.build_meta"

[project]
name = "my-app"
version = "0.0.0"
```

### src/my-app/setup.py
This step is optional but here for completeness to prove you can use the extension.

```python
import setuptools  # noqa: F401 isort:skip must import before distutils
from distutils.command.build import build as _BuildCommand
from setuptools import setup
from setuptools.command.develop import develop as _DevelopCommand
from setuptools.command.sdist import sdist as _SDistCommand


class SDistCommand(_SDistCommand):
    sub_commands = [('build_webassets', None)] + _SDistCommand.sub_commands


class BuildCommand(_BuildCommand):
    sub_commands = [('build_webassets', None)] + _DevelopCommand.sub_commands


class DevelopCommand(_DevelopCommand):
    def run(self):
        self.run_command('build_webassets')
        _DevelopCommand.run(self)


setup(
    cmdclass={
        'sdist': SDistCommand,
        'develop': DevelopCommand,
        'build': BuildCommand,
    }
)
```

## Expected Result

No errors, build the packages and allow my-app to be installed.

## Actual Result

```
❯ uv sync
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.9`.
warning: Missing version constraint (e.g., a lower bound) for `setuptools`
Resolved 4 packages in 2ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: my-app @ file:///Users/michael.merickel/scratch/uv-test/src/my-app
  Caused by: Failed to install requirements from `build-system.requires` (resolve)
  Caused by: No solution found when resolving: setuptools, wheel, my-setuptools-extension
  Caused by: Because my-setuptools-extension was not found in the package registry and you require my-setuptools-extension, we can conclude that your requirements are unsatisfiable.
```

uv is failing to find `my-setuptools-extension` which is defined in the workspace. This is only an issue when it's a build requirement.

## Version Info
```
uv 0.4.6 (Homebrew 2024-09-05)
macos 14.6.1 (m1 max)
```

## Additional Info

In our current repo, we do this by running a specific build step on build-requirement packages into a wheelhouse and then install all of the other packages with a find-links to the wheelhouse.

```
$ env/bin/pip wheel -w wheels src/my-setuptools-extension
$ env/bin/pip install --find-links wheels -e src/my-app
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 23:09_

---

_Label `bug` added by @charliermarsh on 2024-09-06 23:13_

---

_Referenced in [astral-sh/uv#7172](../../astral-sh/uv/pulls/7172.md) on 2024-09-07 17:25_

---

_Closed by @charliermarsh on 2024-10-15 15:31_

---

_Comment by @mmerickel on 2024-10-15 23:36_

Hey @charliermarsh @konstin just to followup on this. I'm super excited that this was merged and it does work if I specify `--wheel` but just to be clear here is the current behavior now which we may want to consider improving or not, but I'll bring it up:

`uv build --project src/my-app` fails because it tries to build the wheel from the source dist and hence the build requirements are not available.

However `uv build --project src/my-app --wheel` works when it builds the wheel directly.

This is good enough for me, in this case I'm only interested in building wheels. I just want to be clear that it's a gotcha. I think it's quite related to what @konstin was getting at with their comment here: https://github.com/astral-sh/uv/pull/7172#issuecomment-2340881869

The output of both is below using uv 0.4.22:

```
~/scratch/uv-test
❯ uvx uv build --project src/my-app
Building source distribution...
warning: Missing version constraint (e.g., a lower bound) for `setuptools`
running egg_info
writing my_app.egg-info/PKG-INFO
writing dependency_links to my_app.egg-info/dependency_links.txt
writing top-level names to my_app.egg-info/top_level.txt
reading manifest file 'my_app.egg-info/SOURCES.txt'
writing manifest file 'my_app.egg-info/SOURCES.txt'
running sdist
running egg_info
writing my_app.egg-info/PKG-INFO
writing dependency_links to my_app.egg-info/dependency_links.txt
writing top-level names to my_app.egg-info/top_level.txt
reading manifest file 'my_app.egg-info/SOURCES.txt'
writing manifest file 'my_app.egg-info/SOURCES.txt'
warning: SDistCommand: standard file not found: should have one of README, README.rst, README.txt, README.md

running build_webassets
running!!!
running check
creating my_app-0.0.0
creating my_app-0.0.0/my_app.egg-info
copying files to my_app-0.0.0...
copying pyproject.toml -> my_app-0.0.0
copying setup.py -> my_app-0.0.0
copying my_app.egg-info/PKG-INFO -> my_app-0.0.0/my_app.egg-info
copying my_app.egg-info/SOURCES.txt -> my_app-0.0.0/my_app.egg-info
copying my_app.egg-info/dependency_links.txt -> my_app-0.0.0/my_app.egg-info
copying my_app.egg-info/top_level.txt -> my_app-0.0.0/my_app.egg-info
copying my_app.egg-info/SOURCES.txt -> my_app-0.0.0/my_app.egg-info
Writing my_app-0.0.0/setup.cfg
Creating tar archive
removing 'my_app-0.0.0' (and everything under it)
Building wheel from source distribution...
error: Failed to resolve requirements from `build-system.requires`
  Caused by: No solution found when resolving: `setuptools`, `wheel`, `my-setuptools-extension`
  Caused by: Because my-setuptools-extension was not found in the package registry and you require my-setuptools-extension, we can conclude that your requirements are unsatisfiable.

~/scratch/uv-test
❯ uvx uv build --project src/my-app --wheel
Building wheel...
warning: Missing version constraint (e.g., a lower bound) for `setuptools`
running egg_info
writing my_app.egg-info/PKG-INFO
writing dependency_links to my_app.egg-info/dependency_links.txt
writing top-level names to my_app.egg-info/top_level.txt
reading manifest file 'my_app.egg-info/SOURCES.txt'
writing manifest file 'my_app.egg-info/SOURCES.txt'
running bdist_wheel
running build
running build_webassets
running!!!
installing to build/bdist.macosx-11.0-arm64/wheel
running install
running install_egg_info
running egg_info
writing my_app.egg-info/PKG-INFO
writing dependency_links to my_app.egg-info/dependency_links.txt
writing top-level names to my_app.egg-info/top_level.txt
reading manifest file 'my_app.egg-info/SOURCES.txt'
writing manifest file 'my_app.egg-info/SOURCES.txt'
Copying my_app.egg-info to build/bdist.macosx-11.0-arm64/wheel/./my_app-0.0.0-py3.9.egg-info
running install_scripts
creating build/bdist.macosx-11.0-arm64/wheel/my_app-0.0.0.dist-info/WHEEL
creating '/Users/michael.merickel/scratch/uv-test/dist/.tmpkc1lKg/.tmp-meam8_7j/my_app-0.0.0-py3-none-any.whl' and adding 'build/bdist.macosx-11.0-arm64/wheel' to it
adding 'my_app-0.0.0.dist-info/METADATA'
adding 'my_app-0.0.0.dist-info/WHEEL'
adding 'my_app-0.0.0.dist-info/top_level.txt'
adding 'my_app-0.0.0.dist-info/RECORD'
removing build/bdist.macosx-11.0-arm64/wheel
Successfully built dist/my_app-0.0.0-py3-none-any.whl
```

---

_Comment by @charliermarsh on 2024-10-15 23:47_

Ahh right. That's a good catch -- that may be what @konstin was getting at (but I misunderstood). I think that's actually good behavior, but it should be documented.

---

_Comment by @kaxil on 2024-10-15 23:55_

@charliermarsh 
Yup, I am fighting that behaviour right now in the Airflow repo: https://github.com/apache/airflow/pull/43056

When I tried to bump uv to `0.4.22` our build from sdist started failing with the following.
```
error: Failed to build: `apache-airflow @ file:///dist/apache_airflow-3.0.0.dev0.tar.gz`
  Caused by: Failed to parse entry for: `local-providers`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

It works if I run `uv pip install --no-sources` but I feel for `uv pip install` should have similar behavior as other tools and maybe explicitly have flags to install things from `[uv.tool.sources]`

---

_Comment by @charliermarsh on 2024-10-15 23:59_

@kaxil -- You're seeing this with `uv build`, or `uv pip install`?

---

_Comment by @kaxil on 2024-10-16 00:01_

Yeah sry, I am seeing the issue with `uv pip install` when installing from sdist.

We have the following in Airflow's `pyproject.toml`:

```toml
...
[tool.uv]
dev-dependencies = [
  "local-providers",
  "apache-airflow-task-sdk"
]

[tool.uv.sources]
# These names must match the names as defined in the pyproject.toml of the workspace items,
# *not* the workspace folder paths
local-providers = { workspace = true }
apache-airflow-task-sdk = { workspace = true }

[tool.uv.workspace]
members = ["providers", "task_sdk"]
```

---

_Comment by @charliermarsh on 2024-10-16 00:07_

And it fails when installing from a `.tar.gz`? I guess I'd consider that a bug. I'll take a look.

---

_Comment by @kaxil on 2024-10-16 00:07_

>And it fails when installing from a .tar.gz? 

Correct.

>I guess I'd consider that a bug. I'll take a look.

Thanks





---

_Comment by @charliermarsh on 2024-10-16 00:12_

@kaxil -- How can I reproduce it?

---

_Comment by @kaxil on 2024-10-16 00:21_

[apache_airflow-3.0.0.dev0.tar.gz](https://github.com/user-attachments/files/17387540/apache_airflow-3.0.0.dev0.tar.gz)

Uploaded the file for you to run `uv pip install` against

---

_Comment by @charliermarsh on 2024-10-16 00:46_

Thank you, it's fixed here: https://github.com/astral-sh/uv/pull/8235

---

_Referenced in [astral-sh/uv#8236](../../astral-sh/uv/issues/8236.md) on 2024-10-16 00:47_

---

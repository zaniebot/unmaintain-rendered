---
number: 2844
title: "re-running \"uv pip install -e ...\" does not re-invoke the build system on the package"
type: issue
state: closed
author: mmerickel
labels:
  - needs-decision
assignees: []
created_at: 2024-04-05T22:22:09Z
updated_at: 2024-09-13T12:56:48Z
url: https://github.com/astral-sh/uv/issues/2844
synced_at: 2026-01-10T01:23:22Z
---

# re-running "uv pip install -e ..." does not re-invoke the build system on the package

---

_Issue opened by @mmerickel on 2024-04-05 22:22_

uv version: 0.1.29
platform: macos 14.4.1 (m1 max)

Setup a simple project using the below `pyproject.toml` and `setup.py` files. With those created, observe some differences between `.venv/bin/pip install -e .` and `uv pip install -e .` in how `setup.py` is invoked:

#### example using pip as expected baseline

```
$ python3 -m venv .venv
$ .venv/bin/pip install -e .
$ cat /tmp/foo
setup.py 1712355322.544586 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'egg_info']
setup.py 1712355323.1131861 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'dist_info', '--output-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-modern-metadata-g5m4gr57', '--keep-egg-info']
setup.py 1712355323.9759429 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'editable_wheel', '--dist-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-wheel-3imq0g2d/.tmp-b981_kcm']
editable_wheel 1712355324.003723
$ .venv/bin/pip install -e .
$ cat /tmp/foo
setup.py 1712355322.544586 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'egg_info']
setup.py 1712355323.1131861 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'dist_info', '--output-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-modern-metadata-g5m4gr57', '--keep-egg-info']
setup.py 1712355323.9759429 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'editable_wheel', '--dist-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-wheel-3imq0g2d/.tmp-b981_kcm']
editable_wheel 1712355324.003723
setup.py 1712355340.953567 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'egg_info']
setup.py 1712355341.528934 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'dist_info', '--output-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-modern-metadata-ipk6za2b', '--keep-egg-info']
setup.py 1712355341.7226088 ['/Users/michael.merickel/scratch/uv-test/.venv/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py', 'editable_wheel', '--dist-dir', '/private/var/folders/0b/gyxh16fx56d7hd_nqbxj1mxc0000gp/T/pip-wheel-_y_0b5q7/.tmp-2qq74dsf']
editable_wheel 1712355341.753067
```

#### example using uv as problematic

```
$ rm -rf myapp.egg-info /tmp/foo .venv
$ uv venv
$ uv pip install -e .
$ cat /tmp/foo
setup.py 1712355432.5460742 ['-c', 'egg_info']
setup.py 1712355432.703499 ['-c', 'editable_wheel', '--dist-dir', '/Users/michael.merickel/Library/Caches/uv/.tmp8VJ4yj/.tmpdHFyuf/.tmp-b485ccgd']
editable_wheel 1712355432.751647
$ uv pip install -e .
$ cat /tmp/foo
setup.py 1712355432.5460742 ['-c', 'egg_info']
setup.py 1712355432.703499 ['-c', 'editable_wheel', '--dist-dir', '/Users/michael.merickel/Library/Caches/uv/.tmp8VJ4yj/.tmpdHFyuf/.tmp-b485ccgd']
editable_wheel 1712355432.751647
```

### Discussion

`uv pip install` is not invoking `editable_wheel` (or any other commands) if it determines that the package is already installed. It only re-invokes things if I change `setup.py` or `pyproject.toml`, but not other files in the project. I have also tested adding a MANIFEST.in and related files, and when I change any of those files the edit is not run.

#### Why is this a problem?

The issue is we are hooking the commands below to invoke other builds (`yarn build` to generate webassets for our projects), and if `uv` does not invoke the install, we have to go into each project and do this, circumventing setuptools as our build system.

`uv` appears to be too aggressive in caching here, it should re-invoke `editable_wheel` on any editable install every time it is passed to `uv pip install -e ...`.

#### Other notes

I tried using `uv pip install --refresh -e .` and it has no effect on the result.

### Repro example files

#### pyproject.toml
```toml
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project]
name = "myapp"
version = "0.0.0"
classifiers = [
    "Private :: Do Not Upload",
]
```

#### setup.py
```python
from setuptools import setup
from setuptools.command.build import build as _BuildCommand
from setuptools.command.develop import develop as _DevelopCommand
from setuptools.command.sdist import sdist as _SDistCommand
from setuptools.command.editable_wheel import editable_wheel as _EditableWheelCommand
import sys
import time


with open('/tmp/foo', 'a+') as fp:
    fp.write(f'setup.py {time.time()} {sys.argv}\n')

class SDistCommand(_SDistCommand):
    def run(self):
        super().run()
        with open('/tmp/foo', 'a+') as fp:
            fp.write(f'sdist {time.time()}\n')


class BuildCommand(_BuildCommand):
    def run(self):
        super().run()
        with open('/tmp/foo', 'a+') as fp:
            fp.write(f'build {time.time()}\n')


class DevelopCommand(_DevelopCommand):
    def run(self):
        super().run()
        with open('/tmp/foo', 'a+') as fp:
            fp.write(f'develop {time.time()}\n')


class EditableWheelCommand(_EditableWheelCommand):
    def run(self):
        super().run()
        with open('/tmp/foo', 'a+') as fp:
            fp.write(f'editable_wheel {time.time()}\n')


setup(
    cmdclass={
        'sdist': SDistCommand,
        'develop': DevelopCommand,
        'build': BuildCommand,
        'editable_wheel': EditableWheelCommand,
    }
)
```

---

_Comment by @charliermarsh on 2024-04-05 22:26_

FWIW you can use `--reinstall` to force a rebuild.

---

_Comment by @mmerickel on 2024-04-05 22:48_

Yeah missed that option - it does work. Guess I'll leave it up to you whether the inconsistency with pip on the caching here is right for `uv` or not. Thank you for the quick response!

---

_Comment by @mmerickel on 2024-04-06 02:12_

To add to that - editable mode is tricky and I’d prefer / expect uv to rebuild it every time. Keep the current logic for non-editable packages such that reinstall isn’t needed. 

---

_Label `needs-decision` added by @charliermarsh on 2024-04-07 00:27_

---

_Comment by @danielhollas on 2024-04-09 08:44_

> To add to that - editable mode is tricky and I’d prefer / expect uv to rebuild it every time. Keep the current logic for non-editable packages such that reinstall isn’t needed.

I'd agree. Supposedly, editable installs are used by developers, and the only time time one needs to run `uv pip install` again on a editable package is when something important changed, right?

---

_Comment by @mmerickel on 2024-04-09 12:58_

That’s the general idea yeah. It’s intended to be used during local development. There’s nothing in the spec to support optimizing an editable install unfortunately.

---

_Referenced in [wntrblm/nox#827](../../wntrblm/nox/issues/827.md) on 2024-05-09 13:45_

---

_Comment by @henryiii on 2024-05-12 06:31_

Running `-e.` (or even `.`) again is a common way to rebuild binary packages, and it's a lot less convenient if it doesn't actually do anything. I don't think local packages should be cached. Local packages tend to have changes without releasing versions (since you are developing on the package).

---

_Referenced in [PyCQA/flake8-pyi#493](../../PyCQA/flake8-pyi/pulls/493.md) on 2024-06-08 16:00_

---

_Comment by @kdeldycke on 2024-07-20 10:14_

I can confirm the behavior with latest `uv`:
```shell-session
$ uv --version
uv 0.2.26 (fe403576c 2024-07-17)
```

I have [a blog](https://github.com/kdeldycke/kevin-deldycke-blog) built with Python-based Pelican and its theme calls [Plumage](https://github.com/kdeldycke/plumage). So when I'm working on it I'd like to tweak stuff on my local dev environment in both repositories.

The [blog has a `pyproject.toml`](https://github.com/kdeldycke/kevin-deldycke-blog/blob/b12e2c4a2bc9fc6219e257dda1bcc4607f498121/pyproject.toml#L28-L53) which boils down to:
```toml
[project]
name = "blog"
dependencies = [
    "pelican [Markdown] ~= 4.9.1",
    "plumage",
]

[tool.uv.sources]
plumage = { path = "../plumage" }
```

To generate the blog with its local modifications, I call `uv run -- pelican`. But it never picks the recent changes in `../plumage`.

The files found in the `blog`'s virtualenv in `./.venv/lib/python3.12/site-packages/plumage/` are not refreshed. And none of the following invocation force its refresh:
```shell-session
$ uv run --refresh-package plumage -- pelican
$ uv run --upgrade -- pelican
$ uv run --upgrade-package plumage -- pelican
$ uv run --no-cache -- pelican
```

My only option is to call:
```shell-session
$ uv run --reinstall-package plumage -- pelican
```

In which case `./.venv/lib/python3.12/site-packages/plumage` become a copy of the local package from `../plumage`.


---

_Comment by @zanieb on 2024-07-20 16:16_

@kdeldycke that is a different issue, you're not using `uv pip install --editable`. Path dependency sources are not editable by default, you need to include `editable = true` in the source entry. 



---

_Comment by @kdeldycke on 2024-07-23 13:50_

> @kdeldycke that is a different issue, you're not using `uv pip install --editable`. Path dependency sources are not editable by default, you need to include `editable = true` in the source entry.

Oh ok I see.

Still, should we requalify my observation into a separate issue? I mean, isn't a path-based local dependency the same as a Git remote branch?

A requirement like `plumage @ git+https://github.com/kdeldycke/plumage.git@main` gets updated each time I call `uv run`:
```shell-session
$ uv run -- pelican
warning: `uv run` is experimental and may change without warning
 Updated https://github.com/kdeldycke/plumage.git (eb17482)
(...)
```

So I was expecting a local-path dependency to be, semantically, the same kind of moving target as a remote `@main` branch.

---

_Referenced in [astral-sh/uv#6822](../../astral-sh/uv/issues/6822.md) on 2024-08-29 16:45_

---

_Comment by @charliermarsh on 2024-09-10 01:44_

We have a new API whereby you can add additional files to consider when invalidating the cache. You can also include the current Git commit (i.e., invalidate whenever the SHA changes).

Looks like this:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = true }]
```

See: https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata.

---

_Comment by @charliermarsh on 2024-09-13 12:56_

Gonna close in favor of #7282.

---

_Closed by @charliermarsh on 2024-09-13 12:56_

---

_Referenced in [astral-sh/uv#7282](../../astral-sh/uv/issues/7282.md) on 2024-10-16 18:18_

---

_Referenced in [astral-sh/uv#12038](../../astral-sh/uv/issues/12038.md) on 2025-03-07 10:52_

---

```yaml
number: 13998
title: What is the proper way to install the current project in editable mode without build isolation?
type: issue
state: open
author: momostein
labels:
  - question
assignees: []
created_at: 2025-06-12T14:21:54Z
updated_at: 2025-06-13T10:19:57Z
url: https://github.com/astral-sh/uv/issues/13998
synced_at: 2026-01-12T16:01:41Z
```

# What is the proper way to install the current project in editable mode without build isolation?

---

_@momostein_

### Question

I'm currently working on some Python Bindings for some C++ code. I'm using `uv`, `scikit-build-core` and `pybind11`.

## Disabling build isolation on my editable project installation improves my development workflow

By default, when using uv and scikit-build-core, build-isolation remains enabled when installing the current project as editable.
However, this means that the whole project has to be recompiled on every change. Also, scikit-build-core import hook does not work with build isolation.

So, to improve compile times and enable automatic rebuilds, I have tried disabling build isolation for the current package. For example, when I'm working on a python package called `package-name` the `pyproject.toml` would looks something like this:

```toml
[build-system]
requires = ["scikit-build-core>=0.11", "pybind11"]
build-backend = "scikit_build_core.build"

[project]
name = "package-name"
# ...

[tool.scikit-build]
editable.verbose = true
editable.rebuild = true

build-dir = "build/{wheel_tag}"

[tool.uv]
# Disable build isolation, but only for the current package in development.
no-build-isolation-package = ["package-name"]
default-groups = "all"

[dependency-groups]
test = [
    "pytest>=8.3.4",
]
build = [
    "pybind11>=2.13.6",
    "scikit-build-core>=0.11.4",
    "setuptools-scm>=8.3.1",
]
```

I now perform the two part installation as such:

```shell
uv sync --no-install-project --only-group build
uv sync
```

For development, this works very well. I can develop my C++ code, then run pytest, and the C++ code will be rebuilt automatically and way faster because it doesn't need to be built from scratch.

Yes, I know that the `cache-keys` setting also works for automatic rebuilding, even with build isolation. However that's still very slow compared to building without isolation.

## Disabling build isolation complicates building wheels for distribution.

Sadly, this comes with a significant drawback. Before disabling build isolation, I can build my package into wheels for any python version without creating a virtual environment. I could just run `uv build -p 3.10` even if my development venv was using python 3.13.

Now, with build isolation disabled on my project, I can't just run `uv build -p 3.10`. I now have to create a separate virtual environment, then install the build dependencies before running `uv build`.

### My current workaround with Nox

So, to automate creation of these virtual environments, I've turned to [Nox](https://nox.thea.codes/en/stable/). This is how such a `noxfile.py` could look:

```python

import nox

# Use uv as the venv backend
nox.options.default_venv_backend = "uv"

PYTHONS = ["3.10", "3.11", "3.12", "3.13"]


@nox.session(python=False, default=False)
def build_sdist(session: nox.Session):
    session.run("uv", "build", "--sdist")


@nox.session(python=PYTHONS, default=False, requires=["build_sdist"])
def build(session: nox.Session):
    """Build all wheels for the current platform.

    Requires the sdist to be present before this session runs.
    """
    session.run_install(
        "uv",
        "sync",
        "--frozen",
        "--no-install-project",
        "--only-group=build",
        f"--python={session.virtualenv.location}",
        env={"UV_PROJECT_ENVIRONMENT": session.virtualenv.location},
    )
    session.run(
        "uv",
        "build",
        "--wheel",
        f"--python={session.virtualenv.location}",
        env={"UV_PROJECT_ENVIRONMENT": session.virtualenv.location},
    )
```

And I run `uv run nox -s build` to build all my wheels.

## Is there a better way?

In my ideal world, this would just be automated by a CI/CD server/pipeline, but I currently do not have access to any CI/CD servers.

Maybe we could add more build isolation options to uv? For example:

 - Add a uv `pyproject.toml` setting to build without isolation, but only on editable installations
 - Add a cli option to `uv build` to re-enable build isolation
   - Even if `no-build-isolation=true` is set in the `pyproject.toml`
   - Currently, `--no-config` seems to work, but that option feels too coarse

What do you think? Does anybody else have any issues with this? Am I actually misusing the no-build-isolation options by using it for my editable project installations?

Also, I must clarify that it is not my intention that the users of my package would need to disable build isolation upon installing/building my package from the sdist. They should be able to build it normally with proper build isolation behavior.

### Platform

Windows 10 x86_64

### Version

uv 0.7.10 (1e5120e15 2025-06-03)

---

_Label `question` added by @momostein on 2025-06-12 14:21_

---

_Comment by @konstin on 2025-06-12 14:41_

Does `uv build --no-config` work for you?

Currently, we don't have a way to make no-build-isolation only apply to editable builds, this may be a possible enhancement.

---

_Comment by @momostein on 2025-06-12 15:25_

Running with `uv build --no-config` does work yes for me, but how would it work for other developers with user-level configuration files?
If I understand the documentation correctly, `--no-config` also disables any user level `uv.toml` configuration files. Right?
I guess that the proof is in the pudding and thus I'll just have to try and see what works.

And yeah, no-build-isolation on editable only would be a nice enhancement.


---

_Comment by @konstin on 2025-06-13 10:19_

> Running with `uv build --no-config` does work yes for me, but how would it work for other developers with user-level configuration files? If I understand the documentation correctly, `--no-config` also disables any user level `uv.toml` configuration files. Right? I guess that the proof is in the pudding and thus I'll just have to try and see what works.

Yes, that's unfortunately true.



---

```yaml
number: 14922
title: "Add --editable flag to `uv build`"
type: pull_request
state: closed
author: adisbladis
labels: []
assignees: []
base: main
head: build-editable
created_at: 2025-07-28T01:24:07Z
updated_at: 2025-07-28T14:32:07Z
url: https://github.com/astral-sh/uv/pull/14922
synced_at: 2026-01-12T16:11:29Z
```

# Add --editable flag to `uv build`

---

_@adisbladis_

## Summary

- Force recompilation of packages installed in editable mode

It can often be useful to force building of packages with native extensions, for example when working on a package with dynamic recompilation managed by import hooks, such as `meson-python` where the underlying build system has it's own build caches that may not always work properly and needs some kicking.

Setuptools used to support `python setup.py build_ext -i` to accomplish this, but in the PEP-517/PEP-660 worlds we don't have an equivalent CLI.

- Use in low level packaging tooling

When building packages with Nix the build process is running inside a sandbox with no file system access and no internet access. This is great for security and reproducibility, but it breaks assumptions in the Python ecosystem:

  - Baking of absolute paths

    Python build systems often (always?) bake in absolute paths pointing to the source directory. where build systems will bake in the absolute path to the build directory.

    This patching is currently handled by a [Python script](https://github.com/pyproject-nix/pyproject.nix/blob/d8355c7/build/hooks/editable_hook/editable_hook/patch_editable.py).

    Note: Script absolute paths are not relevant to this PR, I'm just bringing it up to explain the overall build process of an editable within a Nix context.

  - We want to install the wheel into a custom prefix (the Nix store)

    Within Nix we build each Python package in isolation and install them into their own Nix store prefixes (a requirement for per-package incremental builds). For example the `requests` package in one of my projects is installed into `/nix/store/xhd2c62gj3b5ikwbpsp5kzyb88jc56g5-requests-2.32.3`. This directory contains _only_ the installed files from requests, and not their dependencies.

    In the case of editable packages that means we have to produce a wheel to be able to run `uv pip install --prefix /nix/store/...` on it. Building a wheel is currently handled by a small/janky [Python script](https://github.com/pyproject-nix/pyproject.nix/blob/3db43c7/build/hooks/editable_hook/editable_hook/build_editable.py).

  - Build system side effects

    Some build systems, notably `cython` & `meson-python`, uses [`build_editable`](https://peps.python.org/pep-0660/#build-editable) side effects to place build results in-place into the source tree. Since the editable wheel was built inside the Nix sandbox with a temporary build directory those side effects are discarded.

    This means that we need a mechanism to trigger an editable build, similarly to the "force recompilation" use case outlined above.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Automated test included.
Also extended the `build_wheel` test to ensure that it outputs a regular non-editable wheel.


---

_Assigned to @konstin by @zanieb on 2025-07-28 13:08_

---

_Comment by @konstin on 2025-07-28 13:46_

A problem here is that PEP 660 is very intentional that we must not produce such wheels:

> An “editable” wheel uses the wheel format not for distribution but as ephemeral communication between the build system and the front end. This avoids having the build backend install anything directly. This wheel must not be exposed to end users, nor cached, nor distributed.

Partially, this is a bug in uv cause we sometimes cache editable installs when we shouldn't (instead, we just shouldn't reinstall).

If we want to expose and distribute editable wheels, we should change the spec text to allow this.

> * Some build systems, notably `cython` & `meson-python`, uses [`build_editable`](https://peps.python.org/pep-0660/#build-editable) side effects to place build results in-place into the source tree. Since the editable wheel was built inside the Nix sandbox with a temporary build directory those side effects are discarded.

Using the source tree is a core feature of using editable builds. They are a development feature for where a developer has a repo checked out, either their project or a dependency, and wants to be able to run code and run tests without rebuilding each time. It assumes that the source are not in a temporary build directory, but in the user's workspace. Where this is not the case, regular wheel builds should be preferred over editable builds.

---

_Comment by @adisbladis on 2025-07-28 14:09_

> A problem here is that PEP 660 is very intentional that we must not produce such wheels:

I've read that PEP many times but I completely forgot about that section :/
It's unfortunate that it's worded like that since the PEP authors model of the build process doesn't cover my `Use in low level packaging tooling`.
I'm OK with doing my own thing for this particular use case.

In any case, the key thing I want to achieve here is an equivalent of `python setup.py build_ext -i`: doing the editable side effects without necessarily mutating a virtual environment.
What do you think a good approach to achieve this within `uv` would be?

---

_Comment by @konstin on 2025-07-28 14:18_

I don't think `uv build` counts as low-level tooling - We're intentionally high level, `uv build` just does a smaller task as it exists for `uv publish` (or storing the wheel in some other fashion).

> In any case, the key thing I want to achieve here is an equivalent of `python setup.py build_ext -i`: doing the editable side effects without necessarily mutating a virtual environment.

What about doing a regular wheel build instead and using that? The wheel should contain all files you need to install and run the package, just separated from the source tree.

---

_Comment by @adisbladis on 2025-07-28 14:28_

> I don't think uv build counts as low-level tooling

The "lower level" part is within a distro/meta build system context.

> What about doing a regular wheel build instead and using that? The wheel should contain all files you need to install and run the package, just separated from the source tree.

You're misunderstanding what I'm trying to achieve.

I very much _need_ editable packages.
What is not present in the Python ecosystem today that used to be present when everyone was using setuptools is a way to instruct your build system to "build me my project" while working on something in editable mode without _also_ touching the virtual environment.

---

_Comment by @adisbladis on 2025-07-28 14:32_

In any case I will close this particular PR since it's clearly not getting accepted as-is.

---

_Closed by @adisbladis on 2025-07-28 14:32_

---

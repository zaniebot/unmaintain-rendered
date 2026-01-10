```yaml
number: 6607
title: "Add docs for disabling build isolation with `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/build-docs
created_at: 2024-08-25T14:42:20Z
updated_at: 2025-02-01T02:30:43Z
url: https://github.com/astral-sh/uv/pull/6607
synced_at: 2026-01-10T11:10:34Z
```

# Add docs for disabling build isolation with `uv sync`

---

_Pull request opened by @charliermarsh on 2024-08-25 14:42_

## Summary

This requires some care, so worth documenting the intended workflows.

Closes https://github.com/astral-sh/uv/issues/6437.


---

_Label `documentation` added by @charliermarsh on 2024-08-25 14:42_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-25 14:48_

---

_Comment by @kabouzeid on 2024-08-25 16:31_

Maybe hold off on merging this; there are a couple of problems. Give me an hour or so, and I will provide you with an example.

---

_Comment by @kabouzeid on 2024-08-25 17:10_

First of all, the `--no-build-isolation-package` option doesn't work for me *at all* with any package, whether in `uv pip install`, `uv add`, or in `pyproject.toml`. However, this is a separate issue, so here I will just use `--no-build-isolation` instead.

I see the following issues with the provided example:

1. It only works if `uv.lock` already exists and/or `flash-attn` is already cached. Otherwise, for some reason, `uv` always tries to build `flash-attn` during `sync`, even though it was not requested.

```console
➜ uv sync --extra build
⠴ flash-attn==2.6.3
error: Failed to download and build `flash-attn==2.6.3`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpJtZIFU/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpJtZIFU/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpJtZIFU/lib/python3.12/site-packages/setuptools/build_meta.py", line 502, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpJtZIFU/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 21, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that flash-attn==2.6.3 depends on torch, but doesn't declare it as a build dependency. If flash-attn==2.6.3 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

2. `--no-build-isolation` doesn't install any declared build dependencies. These dependencies also need to be installed to compile `flash-attn`. For `flash-attn`, this is just `psutil`. If the default `hatchling` build system is used, then `hatchling` and `editables` are also needed for building. Those are the dependencies that should go into the `build` group, as they are only required for building and can be removed afterwards.

3. `torch` will almost always be a main dependency, which makes the example a bit confusing. To clarify: `torch` is *also* needed for building extensions, but you wouldn't want to remove `torch` after building the extension.

---

Putting all of this together, it should look more like this

```toml title="pyproject.toml"
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["torch"]  # torch is almost always a main dependency

[project.optional-dependencies]
build = [
    "psutil",
    # "editables",  # only needed if build system is hatchling
    # "hatchling",  # only needed if build system is hatchling
]
compile = ["flash-attn"]

[tool.uv]
no-build-isolation-package = ["flash-attn"]  # currently doesn't work. must be a bug.
```

```console
$ uv sync --extra build  # installs torch + flash-attn build deps
$ uv sync --extra compile  # compiles flash-attn, removes build deps (but keeps torch)
```

Again, this doesn't actually work right now if `uv.lock` is not present, because in that case, `uv` tries to build (but not install) all extras, regardless. Also, since `--no-build-isolation-package` doesn't seem to be working currently, you would still need to pass `--no-build-isolation` for now.


---

_Comment by @charliermarsh on 2024-08-25 17:21_

That first issue is already fixed on main (#6605).

---

_Comment by @charliermarsh on 2024-08-25 17:23_

> It only works if uv.lock already exists and/or flash-attn is already cached. Otherwise, for some reason, uv always tries to build flash-attn during sync, even though it was not requested.

This is expected _and_ required. `flash-attn` ships as a source distribution, and only at `Metadata-Version: 2.1`, so you _must_ ask the build backend for its dependencies per the spec -- which in turn requires that its build dependencies are already installed. There's really nothing we can do about this -- it's a problem with the package. They need to upgrade to `Metadata-Version: 2.2`.

The only way to get things working in this case would be to use the `uv pip install` hack to pre-populate the virtual environment.

I'll have to use a different example here.

---

_Comment by @charliermarsh on 2024-08-25 17:24_

> --no-build-isolation doesn't install any declared build dependencies.

Correct, all sounds right to me.

---

_Comment by @kabouzeid on 2024-08-25 19:12_

> This is expected _and_ required. `flash-attn` ships as a source distribution, and only at `Metadata-Version: 2.1`, so you _must_ ask the build backend for its dependencies per the spec -- which in turn requires that its build dependencies are already installed. There's really nothing we can do about this -- it's a problem with the package. They need to upgrade to `Metadata-Version: 2.2`.

Thank you for the explanation, TIL! It seems the lock file has to be compiled with respect to all extras at once, even if they aren't explicitly requested. This is because the extras can introduce additional constraints on the other dependencies, correct?

---

Somehow all torch extensions I looked at are `Metadata-Version: 2.1`, not sure why. Perhaps we should just use the steps to build the environment in the example then?

```
uv add torch
uv add --optional build psutil  # possibly also others such as editables, hatchling
uv add --optional compile flash-attn
```

Now that the lock file exists and is commited to the repo, the end user can just sync:
```
uv sync --extra build
uv sync --extra compile
```

Actually, it seems that in the first example above the lock file doesn't match the pyproject.toml without another `lock` or `sync` call after adding the packages (see https://github.com/astral-sh/uv/issues/6614).


---

_Comment by @charliermarsh on 2024-08-25 20:03_

> It seems the lock file has to be compiled with respect to all extras at once, even if they aren't explicitly requested. This is because the extras can introduce additional constraints on the other dependencies, correct?

Yeah that's right -- we have to make sure that the entire environment can be installed (including any extras) after locking, so we generate a single lock that tracks all extras, etc.

---

_Comment by @charliermarsh on 2024-08-25 20:09_

I'm sorry that you've been a test subject for all the build isolation stuff -- we only added it at the very end (to `uv sync` -- `uv pip` has had it for a long time) to unblock folks, but we always want (and still want) to come up with a totally different solution to the underlying problems. E.g., I'd prefer an API where you can define the build dependencies yourself, or similar, and then we install in order as a graph.

---

_@zanieb reviewed on 2024-08-26 17:00_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:324 on 2024-08-26 17:00_

Should we repeat the `no-build-isolation-package = ["flash-attn"]` bit here? I was surprised it was omitted.

---

_@zanieb approved on 2024-08-26 17:00_

---

_Merged by @charliermarsh on 2024-08-26 18:21_

---

_Closed by @charliermarsh on 2024-08-26 18:21_

---

_Branch deleted on 2024-08-26 18:21_

---

_Comment by @kabouzeid on 2024-08-27 09:33_

> I'm sorry that you've been a test subject for all the build isolation stuff -- we only added it at the very end (to `uv sync` -- `uv pip` has had it for a long time) to unblock folks, but we always want (and still want) to come up with a totally different solution to the underlying problems. E.g., I'd prefer an API where you can define the build dependencies yourself, or similar, and then we install in order as a graph.

No worries! I'm actually pretty happy that it's working at all with these kinds of dependencies. I've never gotten it to properly work with any tool other than pip.

I totally agree that to really solve this, we need to tackle the actual underlying problems. It's super exciting that this is on the roadmap! I'm keeping a close eye on the project and will provide feedback on how these future APIs handle tricky cases like PyTorch and CUDA extensions. Thanks for all the great work you're doing!

---

_Comment by @tmm1 on 2025-01-24 00:42_

> This is expected _and_ required. `flash-attn` ships as a source distribution, and only at `Metadata-Version: 2.1`, so you _must_ ask the build backend for its dependencies per the spec -- which in turn requires that its build dependencies are already installed. There's really nothing we can do about this -- it's a problem with the package. They need to upgrade to `Metadata-Version: 2.2`.

Has this been surfaced to them? How is this version generally set by a python package? Do they need to bump some build tooling deps?

---

_Comment by @charliermarsh on 2025-01-24 00:50_

It's determined by the build backend. It looks like they're using the version of setuptools that comes on the GitHub Runner: https://github.com/Dao-AILab/flash-attention/actions/runs/12215254337/job/34085814635#step:4:18. If they upgrade setuptools, I would expect it to write Metadata 2.2.

---

_Comment by @charliermarsh on 2025-01-24 00:51_

It might be as simple as changing the `pip install` in that CI step to `pip install -U` or similar.

---

_Comment by @tmm1 on 2025-01-24 02:08_

Thanks. I will work with upstream to fix this. I see they're using setuptools==65.5.0 for the source package, and setuptools==68.0.0 for the cuda binary wheels. What is the minimum required version for Metadata 2.2?

EDIT: [v75.8.0](https://github.com/pypa/setuptools/releases/tag/v75.8.0) contains https://github.com/pypa/setuptools/commit/f285d01e2661b01e4947a4dca7704790b65f2967 which was merged two weeks ago

---

_Comment by @charliermarsh on 2025-01-24 02:13_

(Posting anyway even though you beat me to it) I don't know off-hand but it looks like it was bumped [here](https://github.com/pypa/setuptools/pull/4698) so it honestly might only be the latest version (`75.8.0`).

---

_Comment by @charliermarsh on 2025-02-01 02:30_

It looks like this is out now with the latest `flash-attn`!

---

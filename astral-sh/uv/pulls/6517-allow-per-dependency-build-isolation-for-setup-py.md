```yaml
number: 6517
title: Allow per dependency build isolation for setup.py projects as well
type: pull_request
state: merged
author: tdejager
labels:
  - bug
assignees: []
merged: true
base: main
head: feat/allow-build-isolation-setup-py
created_at: 2024-08-23T14:03:43Z
updated_at: 2024-08-26T14:41:28Z
url: https://github.com/astral-sh/uv/pull/6517
synced_at: 2026-01-10T13:09:51Z
```

# Allow per dependency build isolation for setup.py projects as well

---

_Pull request opened by @tdejager on 2024-08-23 14:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This changes the behavior a bit of the per-dependency build-isolation override. That, if the dist name is known, it is passed into the `SourceBuild::Setup` function. This allows for this override to work for projects without a `pyproject.toml`, like `detectron2`, using the specified requirement name. Previously only the `pyproject.toml` name could be used, which these projects are lacking. An example of a use-case is given in the *Test Plan* section.

Additionally, the `no_build_isolation_package` has been adding to `InstallerSettingsRef` and used in `sync` and other commands, as this was not done yet.

This is useful if you want to **non**-isolate a single package, even ones without a proper `pyproject.toml`


## Test Plan

<!-- How was it tested? -->

With the following pyproject.toml.

```toml
[project]
name = "detectron-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "detectron2",
    "setuptools",
    "torch",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2", rev = "bcfd464d0c810f0442d91a349c0f6df945467143" }

[tool.uv]
no-build-isolation-package = ["detectron2"]
```

The package `detectron2` is now correctly **non**-isolated. Before, because the logic depended on getting the name from the `pyproject.toml`, which is lacking in detectron2 you would get the message, that the source could not be built. This was because it would still be *isolated* in that case.

With these changes you can now install using (given that you are inside a workspace with a venv):

```
uv pip install torch setuptools
uv sync
```

This would previously fail with something like:

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: detectron2 @ git+https://github.com/facebookresearch/detectron2@bcfd464d0c810f0442d91a349c0f6df945467143
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/tdejager/Library/Caches/uv/builds-v0/.tmptloDcZ/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/tdejager/Library/Caches/uv/builds-v0/.tmptloDcZ/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/tdejager/Library/Caches/uv/builds-v0/.tmptloDcZ/lib/python3.12/site-packages/setuptools/build_meta.py", line 502, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/tdejager/Library/Caches/uv/builds-v0/.tmptloDcZ/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 10, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that detectron2 @ git+https://github.com/facebookresearch/detectron2@bcfd464d0c810f0442d91a349c0f6df945467143 depends on torch, but doesn't declare it as a build dependency. If detectron2 @ git+https://github.com/facebookresearch/detectron2@bcfd464d0c810f0442d91a349c0f6df945467143 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
  ```

**Edit**:

Some wording, used isolated where it should be **non**-isolated.




---

_Comment by @zanieb on 2024-08-23 14:08_

Hey!

---

_Comment by @zanieb on 2024-08-23 14:08_

@konstin could you review?

---

_Review requested from @konstin by @konstin on 2024-08-26 08:22_

---

_Comment by @tdejager on 2024-08-26 08:33_

Last changes I did is integrate code from: #6605, was esentially doing a bunch of the same changes.

---

_Comment by @charliermarsh on 2024-08-26 12:37_

Generally makes sense to me, thanks Tim.

---

_@konstin approved on 2024-08-26 12:50_

---

_Label `bug` added by @konstin on 2024-08-26 14:41_

---

_Renamed from "feat: allow per dependency build isolation for setup.py projects as well" to "Allow per dependency build isolation for setup.py projects as well" by @konstin on 2024-08-26 14:41_

---

_Merged by @konstin on 2024-08-26 14:41_

---

_Closed by @konstin on 2024-08-26 14:41_

---

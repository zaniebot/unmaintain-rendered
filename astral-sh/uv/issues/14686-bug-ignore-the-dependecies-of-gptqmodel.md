```yaml
number: 14686
title: "[Bug] Ignore the dependecies of gptqmodel"
type: issue
state: closed
author: shenmishajing
labels:
  - bug
  - external
assignees: []
created_at: 2025-07-17T18:08:39Z
updated_at: 2025-08-01T10:10:04Z
url: https://github.com/astral-sh/uv/issues/14686
synced_at: 2026-01-10T03:32:45Z
```

# [Bug] Ignore the dependecies of gptqmodel

---

_Issue opened by @shenmishajing on 2025-07-17 18:08_

### Summary

uv ignore the dependecies of gptqmodel when install it, leading to import error.

To reproduce:
- use `uv pip install gptqmodel` to install gptqmodel via uv.
- use `uv pip list` to confirm that the dependecies of uv, such as `logbar` are not installed.
- try to improt gptqmodel in python via `import gptqmodel` got error: `No module named 'logbar'`.

gptqmodel has already claimed their dependencies in their setup.py:
https://github.com/ModelCloud/GPTQModel/blob/ca9d634db6a933cfae2c2d8e8be3fe78f76b802d/setup.py#L346
and has logbar in their dependencies:
https://github.com/ModelCloud/GPTQModel/blob/ca9d634db6a933cfae2c2d8e8be3fe78f76b802d/requirements.txt#L16

We can also check that via importlib.

```python
import importlib.metadata
meta = importlib.metadata.metadata('gptqmodel')
print(meta.get_all("Requires-Dist"))
```

The expeced behaviour should be that when uv install gptqmodel, it should also install its dependencies.

### Platform

Linux 5.10.237, Darwin 24.5.0 arm64

### Version

uv 0.7.22

### Python version

Python 3.12.11

---

_Label `bug` added by @shenmishajing on 2025-07-17 18:08_

---

_Comment by @zanieb on 2025-07-17 18:28_

Please share verbose logs of the installation per #9452

---

_Comment by @shenmishajing on 2025-07-17 22:39_

Well, I have tried it again, but it can not be reproduced. It seems a bug of pixi. I have created a pr https://github.com/prefix-dev/pixi/issues/4158 in their repo.

---

_Closed by @zanieb on 2025-07-17 22:43_

---

_Comment by @zanieb on 2025-07-17 22:43_

Thanks for following up!

---

_Label `external` added by @zanieb on 2025-07-17 22:43_

---

_Comment by @lucascolley on 2025-08-01 10:10_

@zanieb I seem to have reproduced the original problem, here is the verbose log. Would you be able to check whether the problem is in GPTQModel or uv?

<details> <summary> log </summary>

```console
sandbox/pixi4158/repro 
❯ uv init; uv add gptqmodel --verbose
Initialized project `repro`
DEBUG uv 0.8.4
DEBUG Found project root: `/Users/lucascolley/sandbox/pixi4158/repro`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/lucascolley/sandbox/pixi4158/repro`
DEBUG Reading Python requests from version file at `/Users/lucascolley/sandbox/pixi4158/repro/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/lucascolley/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.11-macos-aarch64-none`
DEBUG Found `cpython-3.12.11-macos-aarch64-none` at `/Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.11
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
DEBUG Using base executable for virtual environment: /Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
DEBUG Released lock at `/var/folders/_j/kscn3hgd6x995v65h1b3gnrm0000gn/T/uv-d5fe15e95a513546.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: repro @ file:///Users/lucascolley/sandbox/pixi4158/repro
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: repro*
DEBUG Searching for a compatible version of repro @ file:///Users/lucascolley/sandbox/pixi4158/repro (*)
DEBUG Adding direct dependency: gptqmodel*
DEBUG Found fresh response for: https://pypi.org/simple/gptqmodel/
DEBUG Searching for a compatible version of gptqmodel (*)
DEBUG Selecting: gptqmodel==2.2.0 [compatible] (gptqmodel-2.2.0.tar.gz)
DEBUG Acquired lock for `/Users/lucascolley/.cache/uv/sdists-v9/pypi/gptqmodel/2.2.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/d5/1bf44ba82226e3f81e875415a0c27ab786d2fc0d4bbdde67d797eebb3266/gptqmodel-2.2.0.tar.gz
DEBUG No `pyproject.toml` available for: gptqmodel==2.2.0
DEBUG Found static `PKG-INFO` for: gptqmodel==2.2.0
DEBUG Released lock at `/Users/lucascolley/.cache/uv/sdists-v9/pypi/gptqmodel/2.2.0/.lock`
DEBUG Tried 2 versions: gptqmodel 1, repro 1
DEBUG all marker environments resolution took 0.010s
Resolved 2 packages in 24ms
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: repro @ file:///Users/lucascolley/sandbox/pixi4158/repro
DEBUG No workspace root found, using project root
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `repro==0.1.0`
  Requested: {Requirement { name: PackageName("gptqmodel"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "2.2.0" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("gptqmodel"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: repro*
DEBUG Searching for a compatible version of repro @ file:///Users/lucascolley/sandbox/pixi4158/repro (*)
DEBUG Adding direct dependency: gptqmodel>=2.2.0
DEBUG Searching for a compatible version of gptqmodel (>=2.2.0)
DEBUG Selecting: gptqmodel==2.2.0 [preference] (gptqmodel-2.2.0.tar.gz)
DEBUG Tried 2 versions: gptqmodel 1, repro 1
DEBUG all marker environments resolution took 0.000s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: gptqmodel==2.2.0
DEBUG Acquired lock for `/Users/lucascolley/.cache/uv/sdists-v9/pypi/gptqmodel/2.2.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/d5/1bf44ba82226e3f81e875415a0c27ab786d2fc0d4bbdde67d797eebb3266/gptqmodel-2.2.0.tar.gz
   Building gptqmodel==2.2.0
DEBUG Building: gptqmodel==2.2.0
DEBUG Not using uv build backend direct build of gptqmodel==2.2.0, no pyproject.toml: failed to open file `/Users/lucascolley/.cache/uv/sdists-v9/pypi/gptqmodel/2.2.0/2pnv7oQ2RId9rZ7jAdffg/src/pyproject.toml`: No such file or directory (os error 2)
DEBUG Assessing Python executable as base candidate: /Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
DEBUG Reusing existing build environment for: gptqmodel==2.2.0
DEBUG Assessing Python executable as base candidate: /Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
DEBUG Using base executable for virtual environment: /Users/lucascolley/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12.11
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.002s
DEBUG Installing in setuptools==80.9.0 in /Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Installing build requirement: setuptools==80.9.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 14, in <module>
DEBUG   File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
DEBUG     return self._get_build_requires(config_settings, requirements=[])
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
DEBUG     self.run_setup()
DEBUG   File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 512, in run_setup
DEBUG     super().run_setup(setup_script=setup_script)
DEBUG   File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 317, in run_setup
DEBUG     exec(code, locals())
DEBUG   File "<string>", line 24, in <module>
DEBUG ModuleNotFoundError: No module named 'torch'
DEBUG Released lock at `/Users/lucascolley/.cache/uv/sdists-v9/pypi/gptqmodel/2.2.0/.lock`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
  × Failed to build `gptqmodel==2.2.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
          self.run_setup()
        File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 512, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/Users/lucascolley/.cache/uv/builds-v0/.tmplLA51s/lib/python3.12/site-packages/setuptools/build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 24, in <module>
      ModuleNotFoundError: No module named 'torch'

      hint: This error likely indicates that `gptqmodel@2.2.0` depends on `torch`, but doesn't declare it as a build dependency. If `gptqmodel` is a first-party package, consider adding `torch` to its
      `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
DEBUG Released lock at `/Users/lucascolley/sandbox/pixi4158/repro/.venv/.lock`
```

</details>

---

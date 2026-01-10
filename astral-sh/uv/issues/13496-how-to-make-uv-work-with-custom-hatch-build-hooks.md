---
number: 13496
title: "How to make `uv` work with custom `hatch` build hooks"
type: issue
state: closed
author: ma-sadeghi
labels:
  - question
assignees: []
created_at: 2025-05-16T19:15:36Z
updated_at: 2025-05-17T02:29:28Z
url: https://github.com/astral-sh/uv/issues/13496
synced_at: 2026-01-10T01:25:34Z
---

# How to make `uv` work with custom `hatch` build hooks

---

_Issue opened by @ma-sadeghi on 2025-05-16 19:15_

### Question

Hi `uv` devs!

We have a Python package that uses Hatch as the build backend, with a custom build hook that compiles C extensions.

When we manually activate the uv-managed virtual environment and run hatch build, everything works fine. However, running uv build hangs indefinitely.

Here's the debug output leading up to the hang:

```shell
DEBUG uv 0.7.1
DEBUG Found workspace root: `/home/amin/Code/porespy`
DEBUG Adding root workspace member: `/home/amin/Code/porespy`
DEBUG No Python version file found in ancestors of working directory: /home/amin/Code/porespy
DEBUG Searching for Python >=3.10, <3.13 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/amin/Code/porespy/.venv/bin/python3` (virtual environment)
DEBUG Using request timeout of 30s
Building source distribution...
DEBUG No workspace root found, using project root
DEBUG Assessing Python executable as base candidate: /home/amin/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /home/amin/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/20/4d324d65cc6d9205fabedc306948156824eb9f0ee1633355a8f7ec5c66bf/pluggy-1.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/ef/c6deb083748be3bcad6f471b6ae983950c161890bf5ae1b2af80cc56c530/trove_classifiers-2025.5.9.12-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.6.0 [compatible] (pluggy-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.5.9.12 [compatible] (trove_classifiers-2025.5.9.12-py3-none-any.whl)
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.003s
DEBUG Installing in pluggy==1.6.0, hatchling==1.27.0, trove-classifiers==2025.5.9.12, packaging==25.0, pathspec==0.12.1 in /home/amin/.cache/uv/builds-v0/.tmpCoruJb
DEBUG Registry requirement already cached: pluggy==1.6.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.5.9.12
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Installing build requirements: pluggy==1.6.0, hatchling==1.27.0, trove-classifiers==2025.5.9.12, packaging==25.0, pathspec==0.12.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_sdist()`
DEBUG No workspace root found, using project root
DEBUG Calling `hatchling.build.build_sdist("/home/amin/Code/porespy/dist", {})`
Building wheel from source distribution...
DEBUG No workspace root found, using project root
DEBUG Assessing Python executable as base candidate: /home/amin/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /home/amin/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: hatchling*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.6.0 [compatible] (pluggy-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.5.9.12 [compatible] (trove_classifiers-2025.5.9.12-py3-none-any.whl)
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.001s
DEBUG Installing in pluggy==1.6.0, hatchling==1.27.0, trove-classifiers==2025.5.9.12, packaging==25.0, pathspec==0.12.1 in /home/amin/.cache/uv/builds-v0/.tmpXJUzZC
DEBUG Registry requirement already cached: pluggy==1.6.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.5.9.12
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Installing build requirements: pluggy==1.6.0, hatchling==1.27.0, trove-classifiers==2025.5.9.12, packaging==25.0, pathspec==0.12.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG No workspace root found, using project root
DEBUG Calling `hatchling.build.build_wheel("/home/amin/Code/porespy/dist", {}, None)`
Setting up build directory...
warning: `VIRTUAL_ENV=/home/amin/.cache/uv/builds-v0/.tmpXJUzZC` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Using CPython 3.12.9
Creating virtual environment at: .venv
   Building porespy @ file:///home/amin/.cache/uv/sdists-v9/.tmpTJUHUk/porespy-3.0.0a0.dev29
```

Any idea what might be causing this? Is uv build expected to behave differently from hatch build when custom build hooks are involved?

Thanks,
Amin

PS. In case it helps debugging the issue, here's the custom hatch hook (`hatch_build.py`):

```python
import os
import subprocess
import sys
from hatchling.builders.hooks.plugin.interface import BuildHookInterface

class CustomBuildHook(BuildHookInterface):
    def initialize(self, version, build_data):
        build_data['infer_tag'] = True  # Ensure platform-specific wheel tag

        # Determine platform and choose the right build script
        if os.name == "nt":
            script = "./scripts/build.bat"
        else:
            script = "./scripts/build.sh"

        print(f"[hook] Running build script: {script}")
        result = subprocess.run(script, shell=True)

        if result.returncode != 0:
            sys.exit(f"[hook] Build script failed with exit code {result.returncode}")
```

I suspect `subprocess.run(script, shell=True)` is the culprit?

### Platform

Linux 6.12.10-76061203-generic x86_64 GNU/Linux

### Version

uv 0.7.1

---

_Label `question` added by @ma-sadeghi on 2025-05-16 19:15_

---

_Comment by @charliermarsh on 2025-05-16 19:55_

What does `python -m build` do (with https://github.com/pypa/build)?

---

_Comment by @ma-sadeghi on 2025-05-16 20:12_

Thanks for the quick rely! 

It gets stuck similar to `uv build`:

```shell
❯ python -m build
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - hatchling
* Getting build dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - hatchling
* Getting build dependencies for wheel...
* Building wheel...
[hook] Running build script: ./scripts/build.sh
Setting up build directory...
warning: `VIRTUAL_ENV=/home/amin/Code/porespy/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Using CPython 3.12.9
Creating virtual environment at: .venv
   Building porespy @ file:///tmp/build-via-sdist-lsm20yy1/porespy-3.0.0a0.dev29
⠸ Preparing packages... (0/1)                                                                    
```

---

_Comment by @ma-sadeghi on 2025-05-17 02:29_

I don't think it's a `uv` thing, I think I might have set up the build system incorrectly. I did it from scratch and `uv build` works just fine.

Sorry for the false alarm!

---

_Closed by @ma-sadeghi on 2025-05-17 02:29_

---

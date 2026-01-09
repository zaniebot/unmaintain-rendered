---
number: 12310
title: "Inconsistent handling of relative paths in `uv build` with Hatch"
type: issue
state: closed
author: constantin-huetterer
labels:
  - bug
  - external
assignees: []
created_at: 2025-03-19T10:43:50Z
updated_at: 2026-01-02T16:19:17Z
url: https://github.com/astral-sh/uv/issues/12310
synced_at: 2026-01-07T13:12:18-06:00
---

# Inconsistent handling of relative paths in `uv build` with Hatch

---

_Issue opened by @constantin-huetterer on 2025-03-19 10:43_

### Summary

Thank you very much for creating UV! It is a joy to use! ❤ 

There seems to be an inconsistency of how relative paths inside the pyproject.toml of a package are handled between `uv build --package X` and `uv build --package X [--wheel/--sdist]`.

### Context
We are using UV with a MonoRepo including lots of libraries. To avoid duplication in the authors section of each pyproject.toml within our lib folder, we would like to generate this data dynamically from a single source of truth. This is possible with [MetaDataHooks built into Hatch](https://hatch.pypa.io/1.13/how-to/config/dynamic-metadata/).

Our project structure looks roughly like this:
```bash
.
├── README.md
├── hatch_build.py # used to fetch dynamic meta-data at build time
├── lib # lots of libraries in here
│   └── a
│       ├── README.md
│       ├── pyproject.toml # duplicated authors, reference to hatch_build.py
│       └── src
│           └── a
│               ├── __init__.py
│               └── py.typed
#   a lot more libs ...
├── main.py
├── pyproject.toml
└── uv.lock
```

### Bug
When using relative paths inside `lib/a/pyproject.toml` to reference the hatch_build.py, the command `uv build --package a` fails, claiming that it is unable to find hatch_build.py. 

<details>
<summary>Verbose Error Log</summary>

 build --package a --verbose
DEBUG uv 0.6.8 (c1ef48276 2025-03-18)
DEBUG Found workspace root: `REDACTED/uv_bug_reproduction`
DEBUG Adding root workspace member: `REDACTED/uv_bug_reproduction`
DEBUG Adding discovered workspace member: `REDACTED/uv_bug_reproduction/lib/a`
DEBUG Reading Python requests from version file at `REDACTED/uv_bug_reproduction/.python-version`
DEBUG Searching for Python 3.9 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.9.19-macos-aarch64-none` at `REDACTED/uv_bug_reproduction/.venv/bin/python3` (virtual environment)
DEBUG Using request timeout of 30s
Building source distribution...
DEBUG Found workspace root: `REDACTED/uv_bug_reproduction`
DEBUG Adding root workspace member: `REDACTED/uv_bug_reproduction`
DEBUG Adding discovered workspace member: `REDACTED/uv_bug_reproduction/lib/a`
DEBUG Using base executable for virtual environment: REDACTED/uv_bug_reproduction/.venv/bin/python3
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.9.19
DEBUG Solving with target Python version: >=3.9.19
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: tomli{python_full_version < '3.11'}>=1.2.2
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/tomli/
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/46/52/bc0592d1ac02f93983f5e46b0e4872a0abcc498faa2727ff7c184b6bdb6c/trove_classifiers-2025.3.13.13-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1.2.2)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tomli==2.2.1: tomli==2.2.1
DEBUG Adding transitive dependency for tomli==2.2.1: tomli{python_full_version < '3.11'}==2.2.1
DEBUG Searching for a compatible version of tomli (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6e/c2/61d3e0f47e2b74ef40a68b9e6ad5984f6241a942f7cd3bbfbdbd03861ea9/tomli-2.2.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.3.13.13 [compatible] (trove_classifiers-2025.3.13.13-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, tomli 1, trove-classifiers 1
DEBUG marker environment resolution took 0.013s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.3.13.13, packaging==24.2, pathspec==0.12.1, tomli==2.2.1 in REDACTED/.cache/uv/builds-v0/.tmpg5zjKl
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.3.13.13
DEBUG Registry requirement already cached: packaging==24.2
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: tomli==2.2.1
DEBUG Installing build requirements: pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.3.13.13, packaging==24.2, pathspec==0.12.1, tomli==2.2.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_sdist()`
DEBUG Found workspace root: `REDACTED/uv_bug_reproduction`
DEBUG Calling `hatchling.build.build_sdist("REDACTED/uv_bug_reproduction/dist", {})`
Building wheel from source distribution...
DEBUG Found workspace root: `REDACTED/uv_bug_reproduction`
DEBUG Using base executable for virtual environment: REDACTED/uv_bug_reproduction/.venv/bin/python3
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.9.19
DEBUG Solving with target Python version: >=3.9.19
DEBUG Adding direct dependency: hatchling*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: tomli{python_full_version < '3.11'}>=1.2.2
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1.2.2)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tomli==2.2.1: tomli==2.2.1
DEBUG Adding transitive dependency for tomli==2.2.1: tomli{python_full_version < '3.11'}==2.2.1
DEBUG Searching for a compatible version of tomli (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.3.13.13 [compatible] (trove_classifiers-2025.3.13.13-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, tomli 1, trove-classifiers 1
DEBUG marker environment resolution took 0.000s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.3.13.13, packaging==24.2, pathspec==0.12.1, tomli==2.2.1 in REDACTED/.cache/uv/builds-v0/.tmp9gQggn
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.3.13.13
DEBUG Registry requirement already cached: packaging==24.2
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: tomli==2.2.1
DEBUG Installing build requirements: pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.3.13.13, packaging==24.2, pathspec==0.12.1, tomli==2.2.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Found workspace root: `REDACTED/uv_bug_reproduction`
DEBUG Calling `hatchling.build.build_wheel("REDACTED/uv_bug_reproduction/dist", {}, None)`
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/build.py", line 58, in build_wheel
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['standard'])))
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/core.py", line 265, in validate_fields
    _ = self.version
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/core.py", line 149, in version
    self._version = self._get_version()
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/core.py", line 244, in _get_version
    core_metadata = self.core
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/core.py", line 187, in core
    metadata_hooks = self.hatch.metadata.hooks
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/core.py", line 1589, in hooks
    configured_hooks[hook_name] = metadata_hook(self.root, config)
  File "REDACTED/.cache/uv/builds-v0/.tmp9gQggn/lib/python3.9/site-packages/hatchling/metadata/custom.py", line 33, in __new__
    raise OSError(message)
OSError: Build script does not exist: ../../hatch_build.py
  × Failed to build `REDACTED/uv_bug_reproduction/lib/a`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
</details>

Executing the same command with `--wheel` or `--dist` works, producing the source distribution and wheel including the dynamically added authors: 
```bash
➜  uv_bug_reproduction git:(main) ✗ uv build --package a --wheel  
Building wheel...
Successfully built dist/a-0.1.0-py3-none-any.whl
➜  uv_bug_reproduction git:(main) ✗ uv build --package a --sdist
Building source distribution...
Successfully built dist/a-0.1.0.tar.gz
```

Using `uvx hatch build` directly on `lib/a` also produces the desired artifacts. Referencing `hatch_build.py` with absolute paths inside `lib/a/pyproject.toml` also works.

### Reproduction Script
The following shell script will produce a folder named uv_bug_reproduction in the current directory with the project setup described in the Context section. The error can be produced with the `uv build --package a` command inside the directory. 

<details>
<summary>Bash Script</summary>

```bash
#!/bin/bash
uv init --app uv_bug_reproduction
mkdir uv_bug_reproduction/lib
uv init --lib uv_bug_reproduction/lib/a
cd uv_bug_reproduction && uv add lib/a

# add a minimal hatch_build.py to add authors dynamically during the build
cat <<-EOF > ./hatch_build.py
from hatchling.builders.hooks.plugin.interface import BuildHookInterface
from hatchling.metadata.plugin.interface import MetadataHookInterface

class JSONMetaDataHook(MetadataHookInterface):
    def update(self, metadata):
        metadata["authors"] = [
            {"name": "A", "email": "a@example.com"},
            {"name": "B", "email": "b@example.com"},
        ]
EOF

# add the hook to hatch_build.py inside of lib/a/pyproject.toml
cat <<-EOF >> lib/a/pyproject.toml

[tool.hatch.metadata.hooks.custom]
path = "../../hatch_build.py"
EOF

# Replace the static authors string in lib/a/pyproject.toml with a dynamic field
sed -i -e '
/^authors = \[/,/\]/c\
dynamic = ["authors"]
' lib/a/pyproject.toml
```
</details>

### Platform

MacOS Sonoma 14.7.1 (23H222) (ARM64)

### Version

0.6.8 (c1ef48276 2025-03-18)

### Python version

cpython-3.12.9-macos-aarch64-none

---

_Label `bug` added by @constantin-huetterer on 2025-03-19 10:43_

---

_Renamed from "Package build fails when using relative paths for dynamic meta-data with hatch" to "Inconsistent handling of relative paths in `uv build` with Hatch" by @constantin-huetterer on 2025-03-19 10:57_

---

_Comment by @charliermarsh on 2025-03-19 12:40_

This sounds a lot like https://github.com/astral-sh/uv/issues/11557. Does `python -m build` produce the same artifacts as us?

---

_Comment by @constantin-huetterer on 2025-03-19 17:22_

Thank you for the quick reply! 🙏 
I saw this ticket and thought that our problem is different, because the sdist and wheel files contain exactly what we expect (except the dynamic meta-data). However, after having read it with all the comments, I believe that this issue is likely not a problem with UV since `python -m build` indeed also complains about the missing hatch_build.py. Feel free to close this issue.  

However, I would be very grateful if you could point me in the direction of a solution to this problem or help me understand it better. 

If I understand the other discussion correctly, then uv's build frontend builds the source distribution first and then builds the wheel from the source distribution. Hatchlings build-frontend does not do that and this is why `uv run hatch build` works with the same configuration.

To make the pyproject.toml compatible with a non-hatch build-frontend, I tried to force-include the hatch_build.py into the source-distribution.
```toml
[tool.hatch.build.targets.sdist.force-include]
"../../hatch_build.py" =  "hatch_build.py"
``` 
This puts the hatch_build.py in the source distribution as expected. However, if I change the search path accordingly:
```toml
[tool.hatch.metadata.hooks.custom]
path = "hatch_build.py"
# path = "../../hatch_build.py" # old
```
Then the building of the source distribution fails because it does not find the hatch_build.py. 

It seems like I would require the path to be `../../hatch_build.py` for the source distribution and `hatch_build.py` for the wheel.
However this does not seem to be possible with the [Metadata Hook](https://hatch.pypa.io/latest/plugins/metadata-hook/reference/). Do you have another idea?

---

_Comment by @zanieb on 2025-03-19 17:37_

@ofek is the best resource here

---

_Comment by @ofek on 2025-03-19 17:44_

Can you provide something to help reproduce? Any `hatch_build.py` file at the root is always included in the source distribution: https://hatch.pypa.io/latest/plugins/builder/sdist/#default-file-selection

---

_Comment by @constantin-huetterer on 2025-03-19 17:47_

Wow, I am amazed how fast you guys are! Thanks for the help! 🥇 
@ofek you can find a reproduction-script at the bottom of the issue description. 

---

_Comment by @ofek on 2025-03-19 17:52_

I'll debug in a few hours if that's okay with you.

---

_Comment by @constantin-huetterer on 2025-03-25 10:29_

Of course! Thank you, @ofek  🙏 🙂 
If I can assist in any way, please let me know.

---

_Label `external` added by @zanieb on 2025-04-01 18:31_

---

_Comment by @ajjahn on 2025-04-18 16:02_

Hi. I have run into this same problem. I'm not sure this is an external bug as running `hatch build` directly is able to locate the `../../hatch_build.py` file correctly.

---

_Comment by @steven-r on 2025-10-09 08:48_

> Hi. I have run into this same problem. I'm not sure this is an external bug as running `hatch build` directly is able to locate the `../../hatch_build.py` file correctly.

It seems that `uv build` run the build in a different folder than the project folder and therefore relative path names fail. I've added some print statements:

```
OSError: Readme file does not exist: ../../README.md
Root path: /home/xxx/.cache/uv/sdists-v9/.tmp8FdOI3/mybook_lib-2.0.0 
Readme path: .../../README.md
Normalized path: /home/xxx/.cache/uv/sdists-v9/README.md
```

Which is then true as the file does not exist at this location. `hatchling` assumes the build being executed in the project folder. `uv` seems to have a different opinion on this.

---

_Comment by @tmr232 on 2025-10-13 14:39_

Just ran into the same issue.
Running `uv build --wheel` succeeds and `uv build` fails due to the wrong directory path.
I checked, and `python -m build` seems to have the exact same behaviour. 
Only `hatch build` seems to work fine with such paths.

---

_Comment by @charliermarsh on 2025-10-13 16:31_

`python -m build` is effectively the reference implementation. If `python -m build` and `uv build` exhibit the same behavior, then I believe this is a bug that needs to be solved in `hatchling` or in your own configuration.

---

_Comment by @constantin-huetterer on 2026-01-02 16:19_

I agree. Closing this issue. Thanks for looking into it!

---

_Closed by @constantin-huetterer on 2026-01-02 16:19_

---

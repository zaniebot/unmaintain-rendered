```yaml
number: 16758
title: "Running `uv sync --locked` in a Dockerfile with workspace dependencies gives \"the lockfile at `uv.lock` needs to be updated\", even though no update is needed"
type: issue
state: closed
author: hakontonne
labels:
  - bug
  - question
assignees: []
created_at: 2025-11-17T12:02:15Z
updated_at: 2025-11-19T11:12:31Z
url: https://github.com/astral-sh/uv/issues/16758
synced_at: 2026-01-12T16:02:37Z
```

# Running `uv sync --locked` in a Dockerfile with workspace dependencies gives "the lockfile at `uv.lock` needs to be updated", even though no update is needed

---

_@hakontonne_

### Summary

Hello, please see this repo to reproduce the bug:
https://github.com/hakontonne/uv-locking-bug

Essentially, if you have a workspace member in your project and you try to build from the lockfile in a Dockerfile, it fails with the message:

> The lockfile at uv.lock needs to be updated, but --locked was provided. To update the lockfile, run uv lock

But no update is needed.

Possibly related to https://github.com/astral-sh/uv/issues/8581 


### Platform

macOS 25.0.0 arm64

### Version

uv 0.9.8 (85c5d3228 2025-11-07)

### Python version

3.12.2

---

_Label `bug` added by @hakontonne on 2025-11-17 12:02_

---

_Comment by @zanieb on 2025-11-17 14:43_

Can you provide verbose logs (`-vv`) for the `--locked` invocation please? It'll say why the lockfile needs to be updated

---

_Comment by @hakontonne on 2025-11-17 14:59_

Here is the raw output from the run:
```
 > [stage-0 3/5] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock,ro     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --no-install-project --no-dev --locked -vv:
0.089 DEBUG uv 0.9.9
0.089 TRACE Checking shared lock for `/root/.cache/uv` at `/root/.cache/uv/.lock`
0.089 DEBUG Acquired shared lock for `/root/.cache/uv`
0.089 DEBUG Found project root: `/app`
0.089 DEBUG Found workspace root: `/app`
0.089 TRACE Discovering workspace members for: `/app`
0.089 DEBUG Adding root workspace member: `/app`
0.090 TRACE Checking lock for `/app` at `/tmp/uv-1c83b73deef05048.lock`
0.090 DEBUG Acquired lock for `/app`
0.090 DEBUG No Python version file found in workspace: /app
0.090 DEBUG Using Python request `>=3.12` from `requires-python` metadata
0.090 DEBUG Checking for Python environment at: `.venv`
0.090 DEBUG Searching for Python >=3.12 in managed installations or search path
0.090 DEBUG Searching for managed installations at `/root/.local/share/uv/python`
0.091 TRACE Found `ld` path: /lib/ld-linux-aarch64.so.1
0.091 TRACE stdout output from `ld`: ""
0.091 TRACE stderr output from `ld`: "/lib/ld-linux-aarch64.so.1: missing program name\nTry '/lib/ld-linux-aarch64.so.1 --help' for more information.\n"
0.091 TRACE Tried to find musl version by running `"/lib/ld-linux-aarch64.so.1"`, but failed: Could not find musl version in output of: `/lib/ld-linux-aarch64.so.1`
0.091 TRACE Tried to find libc version from possible symlink at "/lib/ld-linux-aarch64.so.1", but failed: Failed to find glibc version in the filename of linker: `aarch64-linux-gnu/ld-linux-aarch64.so.1`
0.092 TRACE stdout output from `ld.so --version`: "ld.so (Debian GLIBC 2.36-9+deb12u13) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
0.092 TRACE Found manylinux 2.36 in stdout of ld.so version
0.092 TRACE Searching PATH for executables: python3, python
0.092 TRACE Checking `PATH` directory for interpreters: /usr/local/bin
0.092 TRACE Found possible Python executable: /usr/local/bin/python3
0.092 TRACE Found cached interpreter info for Python 3.12.12, skipping query of: /usr/local/bin/python3
0.092 DEBUG Found `cpython-3.12.12-linux-aarch64-gnu` at `/usr/local/bin/python3` (first executable in the search path)
0.092 Using CPython 3.12.12 interpreter at: /usr/local/bin/python3
0.092 Creating virtual environment at: .venv
0.092 DEBUG Using base executable for virtual environment: /usr/local/bin/python3
0.093 DEBUG Released lock at `/tmp/uv-1c83b73deef05048.lock`
0.093 TRACE Checking lock for `.venv` at `.venv/.lock`
0.093 DEBUG Acquired lock for `.venv`
0.095 DEBUG Using request timeout of 30s
0.095 DEBUG Resolving despite existing lockfile due to mismatched members:
0.095   Requested: {}
0.095   Existing: {PackageName("examplelib"), PackageName("uv-bug-app")}
0.096 DEBUG Found static `pyproject.toml` for: uv-bug-app @ file:///app
0.096 DEBUG Found workspace root: `/app`
0.096 TRACE Cached workspace members for: `/app`
0.097 TRACE Performing lookahead for uv-bug-app @ file:///app
0.097 DEBUG Solving with installed Python version: 3.12.12
0.097 DEBUG Solving with target Python version: >=3.12
0.097 TRACE Assigned packages: 
0.097 TRACE Chose package for decision: root. remaining choices: 
0.097 DEBUG Adding direct dependency: uv-bug-app*
0.097 TRACE Assigned packages: root==0a0.dev0
0.098 TRACE Chose package for decision: uv-bug-app. remaining choices: 
0.098 DEBUG Searching for a compatible version of uv-bug-app @ file:///app (*)
0.098 DEBUG Adding direct dependency: pydantic>=2.11.7
0.098 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0
0.098 TRACE Chose package for decision: pydantic. remaining choices: 
0.098 TRACE Fetching metadata for pydantic from https://pypi.org/simple/pydantic/
0.099 TRACE Response from https://pypi.org/simple/pydantic/ is storable because it has a 'public' cache-control directive
0.099 TRACE Freshness lifetime found via cache-control max age setting: 600s
0.099 DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
0.099 TRACE Received package metadata for: pydantic
0.099 TRACE Using preference pydantic 2.12.4
0.099 DEBUG Searching for a compatible version of pydantic (>=2.11.7)
0.099 TRACE Using preference pydantic 2.12.4
0.099 DEBUG Selecting: pydantic==2.12.4 [preference] (pydantic-2.12.4-py3-none-any.whl)
0.099 TRACE Response from https://files.pythonhosted.org/packages/82/2f/e68750da9b04856e2a7ec56fc6f034a5a79775e9b9a81882252789873798/pydantic-2.12.4-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
0.099 TRACE Freshness lifetime found via cache-control max age setting: 365000000s
0.099 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/82/2f/e68750da9b04856e2a7ec56fc6f034a5a79775e9b9a81882252789873798/pydantic-2.12.4-py3-none-any.whl.metadata
0.099 TRACE Received built distribution metadata for: pydantic==2.12.4
0.099 DEBUG Adding transitive dependency for pydantic==2.12.4: annotated-types>=0.6.0
0.099 DEBUG Adding transitive dependency for pydantic==2.12.4: pydantic-core>=2.41.5, <2.41.5+
0.099 DEBUG Adding transitive dependency for pydantic==2.12.4: typing-extensions>=4.14.1
0.099 DEBUG Adding transitive dependency for pydantic==2.12.4: typing-inspection>=0.4.2
0.099 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0, pydantic==2.12.4
0.099 TRACE Chose package for decision: pydantic-core. remaining choices: annotated-types, typing-inspection, typing-extensions
0.099 TRACE Fetching metadata for annotated-types from https://pypi.org/simple/annotated-types/
0.099 TRACE Fetching metadata for pydantic-core from https://pypi.org/simple/pydantic-core/
0.100 TRACE Fetching metadata for typing-extensions from https://pypi.org/simple/typing-extensions/
0.100 TRACE Fetching metadata for typing-inspection from https://pypi.org/simple/typing-inspection/
0.101 TRACE Response from https://pypi.org/simple/annotated-types/ is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 600s
0.101 DEBUG Found fresh response for: https://pypi.org/simple/annotated-types/
0.101 TRACE Received package metadata for: annotated-types
0.101 TRACE Response from https://pypi.org/simple/typing-extensions/ is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 600s
0.101 DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
0.101 TRACE Received package metadata for: typing-extensions
0.101 TRACE Response from https://pypi.org/simple/typing-inspection/ is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 600s
0.101 DEBUG Found fresh response for: https://pypi.org/simple/typing-inspection/
0.101 TRACE Received package metadata for: typing-inspection
0.101 TRACE Using preference annotated-types 0.7.0
0.101 TRACE Using preference typing-extensions 4.15.0
0.101 TRACE Using preference typing-inspection 0.4.2
0.101 TRACE Response from https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 365000000s
0.101 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata
0.101 TRACE Received built distribution metadata for: annotated-types==0.7.0
0.101 TRACE Response from https://files.pythonhosted.org/packages/dc/9b/47798a6c91d8bdb567fe2698fe81e0c6b7cb7ef4d13da4114b41d239f65d/typing_inspection-0.4.2-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 365000000s
0.101 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dc/9b/47798a6c91d8bdb567fe2698fe81e0c6b7cb7ef4d13da4114b41d239f65d/typing_inspection-0.4.2-py3-none-any.whl.metadata
0.101 TRACE Received built distribution metadata for: typing-inspection==0.4.2
0.101 TRACE Response from https://files.pythonhosted.org/packages/18/67/36e9267722cc04a6b9f15c7f3441c2363321a3ea07da7ae0c0707beb2a9c/typing_extensions-4.15.0-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
0.101 TRACE Freshness lifetime found via cache-control max age setting: 365000000s
0.101 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/18/67/36e9267722cc04a6b9f15c7f3441c2363321a3ea07da7ae0c0707beb2a9c/typing_extensions-4.15.0-py3-none-any.whl.metadata
0.101 TRACE Received built distribution metadata for: typing-extensions==4.15.0
0.105 TRACE Response from https://pypi.org/simple/pydantic-core/ is storable because it has a 'public' cache-control directive
0.105 TRACE Freshness lifetime found via cache-control max age setting: 600s
0.105 DEBUG Found fresh response for: https://pypi.org/simple/pydantic-core/
0.106 TRACE Received package metadata for: pydantic-core
0.106 DEBUG Searching for a compatible version of pydantic-core (>=2.41.5, <2.41.5+)
0.106 TRACE Using preference pydantic-core 2.41.5
0.106 TRACE Using preference pydantic-core 2.41.5
0.106 DEBUG Selecting: pydantic-core==2.41.5 [preference] (pydantic_core-2.41.5-cp312-cp312-macosx_10_12_x86_64.whl)
0.106 TRACE Response from https://files.pythonhosted.org/packages/5f/5d/5f6c63eebb5afee93bcaae4ce9a898f3373ca23df3ccaef086d0233a35a7/pydantic_core-2.41.5-cp312-cp312-macosx_10_12_x86_64.whl.metadata is storable because it has a 'public' cache-control directive
0.106 TRACE Freshness lifetime found via cache-control max age setting: 365000000s
0.106 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/5d/5f6c63eebb5afee93bcaae4ce9a898f3373ca23df3ccaef086d0233a35a7/pydantic_core-2.41.5-cp312-cp312-macosx_10_12_x86_64.whl.metadata
0.106 TRACE Received built distribution metadata for: pydantic-core==2.41.5
0.106 DEBUG Adding transitive dependency for pydantic-core==2.41.5: typing-extensions>=4.14.1
0.106 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0, pydantic==2.12.4, pydantic-core==2.41.5
0.106 TRACE Chose package for decision: annotated-types. remaining choices: typing-extensions, typing-inspection
0.106 DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
0.106 TRACE Using preference annotated-types 0.7.0
0.106 DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
0.107 TRACE Skipping typing-extensions>=4.0.0 ; python_full_version < '3.9' because of Requires-Python: >=3.12
0.107 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0, pydantic==2.12.4, pydantic-core==2.41.5, annotated-types==0.7.0
0.107 TRACE Chose package for decision: typing-extensions. remaining choices: typing-inspection
0.107 DEBUG Searching for a compatible version of typing-extensions (>=4.14.1)
0.107 TRACE Using preference typing-extensions 4.15.0
0.107 DEBUG Selecting: typing-extensions==4.15.0 [preference] (typing_extensions-4.15.0-py3-none-any.whl)
0.107 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0, pydantic==2.12.4, pydantic-core==2.41.5, annotated-types==0.7.0, typing-extensions==4.15.0
0.107 TRACE Chose package for decision: typing-inspection. remaining choices: 
0.107 DEBUG Searching for a compatible version of typing-inspection (>=0.4.2)
0.107 TRACE Using preference typing-inspection 0.4.2
0.107 DEBUG Selecting: typing-inspection==0.4.2 [preference] (typing_inspection-0.4.2-py3-none-any.whl)
0.107 DEBUG Adding transitive dependency for typing-inspection==0.4.2: typing-extensions>=4.12.0
0.107 TRACE Assigned packages: root==0a0.dev0, uv-bug-app==0.1.0, pydantic==2.12.4, pydantic-core==2.41.5, annotated-types==0.7.0, typing-extensions==4.15.0, typing-inspection==0.4.2
0.107 DEBUG Tried 6 versions: annotated-types 1, pydantic 1, pydantic-core 1, typing-extensions 1, typing-inspection 1, uv-bug-app 1
0.107 DEBUG all marker environments resolution took 0.009s
0.107 TRACE Resolution: ResolverEnvironment { kind: Universal { initial_forks: [], markers: true, include: {}, exclude: {} } }
0.107 TRACE Resolution edge: ROOT -> uv-bug-app
0.107 TRACE Resolution edge:     0a0.dev0 -> 0.1.0
0.107 TRACE Resolution edge: uv-bug-app -> pydantic
0.107 TRACE Resolution edge:     0.1.0 -> 2.12.4
0.107 TRACE Resolution edge: typing-inspection -> typing-extensions
0.107 TRACE Resolution edge:     0.4.2 -> 4.15.0
0.107 TRACE Resolution edge: pydantic-core -> typing-extensions
0.107 TRACE Resolution edge:     2.41.5 -> 4.15.0
0.107 TRACE Resolution edge: pydantic -> annotated-types
0.107 TRACE Resolution edge:     2.12.4 -> 0.7.0
0.107 TRACE Resolution edge: pydantic -> pydantic-core
0.107 TRACE Resolution edge:     2.12.4 -> 2.41.5
0.107 TRACE Resolution edge: pydantic -> typing-extensions
0.107 TRACE Resolution edge:     2.12.4 -> 4.15.0
0.107 TRACE Resolution edge: pydantic -> typing-inspection
0.107 TRACE Resolution edge:     2.12.4 -> 0.4.2
0.107 Resolved 6 packages in 12ms
0.108 The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
0.108 DEBUG Released lock at `/app/.venv/.lock`
0.108 DEBUG Released lock at `/root/.cache/uv/.lock`
```

---

_Comment by @zanieb on 2025-11-17 15:09_

The key lines are

> 0.095 DEBUG Resolving despite existing lockfile due to mismatched members:
> 0.095   Requested: {}
> 0.095   Existing: {PackageName("examplelib"), PackageName("uv-bug-app")}

which is created at https://github.com/astral-sh/uv/blob/ae1edef9c0a4906cd22838b30fd719723c6cb67e/crates/uv-resolver/src/lock/mod.rs#L1472-L1479

You can see the logs from workspace discovery which is a hint at where we're getting the missing values from

> 0.089 DEBUG Found project root: `/app`
> 0.089 DEBUG Found workspace root: `/app`
> 0.089 TRACE Discovering workspace members for: `/app`
> 0.089 DEBUG Adding root workspace member: `/app`

I think you should just use `--frozen` here since you're intentionally not mounting the workspace members. The subsequent sync can use `--locked` still.


---

_Comment by @zanieb on 2025-11-17 15:12_

Or mount your workspace member's `pyproject.toml`

```diff
diff --git a/Dockerfile b/Dockerfile
index 73dec13..e1e06d6 100644
--- a/Dockerfile
+++ b/Dockerfile
@@ -11,6 +11,7 @@ WORKDIR /app
 RUN --mount=type=cache,target=/root/.cache/uv \
     --mount=type=bind,source=uv.lock,target=uv.lock,ro \
     --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
+    --mount=type=bind,source=packages/examplelib/pyproject.toml,target=packages/examplelib/pyproject.toml \
     uv sync --no-install-project --no-dev --locked
 
 # 2) Copy the full source code
```

---

_Label `question` added by @zanieb on 2025-11-17 15:12_

---

_Comment by @hakontonne on 2025-11-19 11:12_

Thanks for the asstance, using `--frozen` did work, but I ended up going with mounting the workspace members' `pyproject.toml`, so we actually check the lockfile properly :) 

Keep up the good work! Its much appricated 

---

_Closed by @hakontonne on 2025-11-19 11:12_

---

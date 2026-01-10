```yaml
number: 15326
title: Document improvements to build-isolation setups
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/build-docs
created_at: 2025-08-16T15:51:58Z
updated_at: 2025-12-22T12:01:37Z
url: https://github.com/astral-sh/uv/pull/15326
synced_at: 2026-01-10T05:49:14Z
```

# Document improvements to build-isolation setups

---

_Pull request opened by @charliermarsh on 2025-08-16 15:51_

## Summary

There's a lot here (maybe it should go somewhere else?), but this is a complex topic and I wanted to include a lot of copy-pasteable examples for common cases.

Closes https://github.com/astral-sh/uv/issues/10694.
Closes https://github.com/astral-sh/uv/issues/13959.
Closes https://github.com/astral-sh/uv/issues/15248.
Closes https://github.com/astral-sh/uv/issues/15316.


---

_Label `documentation` added by @charliermarsh on 2025-08-16 15:52_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-16 16:07_

---

_Marked ready for review by @charliermarsh on 2025-08-16 16:07_

---

_Review comment by @vwxyzjn on `docs/concepts/projects/config.md`:296 on 2025-08-16 17:50_

This doesn't work:

```
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ cat pyproject.toml
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["flash-attn", "torch"]

[tool.uv.extra-build-dependencies]
flash-attn = [{ dependencies = ["torch"], match-runtime = true }]

[tool.uv.extra-build-variables]
flash-attn = { FLASH_ATTENTION_SKIP_CUDA_BUILD = "TRUE" }
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ uv sync
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 10, column 14
     |
  10 | flash-attn = [{ dependencies = ["torch"], match-runtime = true }]
     |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  data did not match any variant of untagged enum ExtraBuildDependencyWire

error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 10, column 14
   |
10 | flash-attn = [{ dependencies = ["torch"], match-runtime = true }]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
data did not match any variant of untagged enum ExtraBuildDependencyWire
```

---

_@vwxyzjn reviewed on 2025-08-16 17:51_

Love the examples! The flash attention one doesn't work, but I tested deep_ep and deep_gemm which all works!

---

_@charliermarsh reviewed on 2025-08-16 21:40_

---

_Review comment by @charliermarsh on `docs/concepts/projects/config.md`:296 on 2025-08-16 21:40_

Oh thank you for trying this, there’s a typo 😩

---

_Comment by @vwxyzjn on 2025-08-16 22:40_

I have put a minimal repro here: https://github.com/vwxyzjn/minimal-uv-deep-ep-gemm-installation/tree/main

---

_@zanieb approved on 2025-08-18 21:15_

I have some minor edits and, as you said, we probably want to find a better home for all this content, but let's just get this in and iterate from there since it's so important.

---

_Merged by @zanieb on 2025-08-18 21:15_

---

_Closed by @zanieb on 2025-08-18 21:15_

---

_Branch deleted on 2025-08-18 21:15_

---

_Comment by @tobiasdiez on 2025-08-19 02:03_

These are very nice improvements. Thanks a lot for everyone that worked on this!

Another use case of "no-build-isolation" of the main project is that some build backends (like python-meson) allow to recompile files on the fly in editable installs. For this to work, naturally the build dependencies of the main project need to be available also at runtime. (More details at https://github.com/astral-sh/uv/issues/10694#issuecomment-2874835809)
It's not clear to me if those recent changes cover this use case as well. One still needs to create an "extra" for the build dependencies, duplicating what is already in `build-system.requires`, and then use `uv sync --extra build`, right? Or is there a way to sync the build requirements of the main project without duplicating them to an extra?

---

_Comment by @charliermarsh on 2025-08-19 09:25_

Thanks @tobiasdiez! I can take a look and add some documentation for that. Can you give me a minimal `pyproject.toml` + a set of commands to reproduce, for testing?

---

_Review comment by @zsol on `docs/concepts/projects/config.md`:326 on 2025-08-20 10:12_

This is a typo
```suggestion
that changes to these settings will trigger a reinstall and rebuild of the affected packages.
```

---

_@zsol reviewed on 2025-08-20 10:12_

---

_@charliermarsh reviewed on 2025-08-20 10:36_

---

_Review comment by @charliermarsh on `docs/concepts/projects/config.md`:326 on 2025-08-20 10:36_

PR!!!

---

_@zsol reviewed on 2025-08-20 11:07_

---

_Review comment by @zsol on `docs/concepts/projects/config.md`:326 on 2025-08-20 11:07_

#15390

---

_@tobiasdiez reviewed on 2025-09-13 00:16_

---

_Review comment by @tobiasdiez on `docs/concepts/projects/config.md`:259 on 2025-09-13 00:16_

Does this also ensure that the package annotated with `match-runtime = true` will only be built once? So in the example below (assuming that torch wouldn't ship wheels), would torch be built once as the build dependency of deepspeed and then another time to be installed in the main env? Or does uv cache the built of torch and reuses it?

I hope it is cached - in this case, `match-runtime` might be also a very useful option to set for packages that take a long time to build. I can open a PR to add this if wished.

---

_Comment by @tobiasdiez on 2025-09-13 01:14_

> Thanks @tobiasdiez! I can take a look and add some documentation for that. Can you give me a minimal `pyproject.toml` + a set of commands to reproduce, for testing?

Sorry for taking so long to come back to you. Here is an example `pyproject.toml` that shows the problem:

```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
[build-system]
build-backend = 'mesonpy'
requires = ["cython>=3.1.3", "meson-python>=0.18.0"]
```

This is just what you get with `uv init` plus adding the meson-python as a build backend. To run `uv sync` you also need a simple `meson.build`, say
```
project(
  'test',
  ['c', 'cpp', 'cython'],
  meson_version: '>=1.2',
)

py_module = import('python')
py = py_module.find_installation(pure: false)

py.install_sources(
  'main.py',
)
```

In a real application you would now actually use cython to compile an extension etc, but for the sake of having a simple example let's ignore that.

After `uv sync` the package is installed in editable mode, and in particular you have a file `.venv\Lib\site-packages\_test_editable_loader.py` which intercepts all imports and rebuilds the project if necessary. This means you need to have the build dependencies also installed at runtime. This is however not the case currently if you run `uv sync`.

Current workaround is:
```
uv pip install meson-python cython
uv sync --no-build-isolation
```

So it's similar to the problems discussed in this PR, but the difference is that the `no-build-isolation` concerns the main project and not one of its dependencies.


Let me know if this is sufficient to understand the problem, or if you would prefer to have a full example that actually compiles something etc.

---

_Comment by @tobiasdiez on 2025-12-22 12:01_

@charliermarsh Do you have any update on how to use build isolation for the main project? We have migrated a few projects to meson recently, and every single one of them faces the problem that one has to manually install meson-python+cython using `uv pip`.

---

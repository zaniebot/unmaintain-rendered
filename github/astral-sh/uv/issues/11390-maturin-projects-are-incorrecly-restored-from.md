---
number: 11390
title: Maturin projects are incorrecly restored from cache in editable mode
type: issue
state: open
author: polw-zi
labels:
  - bug
assignees: []
created_at: 2025-02-10T16:22:48Z
updated_at: 2025-10-23T11:17:12Z
url: https://github.com/astral-sh/uv/issues/11390
synced_at: 2026-01-07T13:12:18-06:00
---

# Maturin projects are incorrecly restored from cache in editable mode

---

_Issue opened by @polw-zi on 2025-02-10 16:22_

### Summary

See [this MRE](https://github.com/user-attachments/files/18737082/maturin-test.zip), which is a toy example of a mixed-language maturin project.

1. Extract the example.
2. Run `uv sync`. This builds the `foo.pyd`, places it into the Python source tree.
3. Create a fresh copy of the example, by deleting the `.venv` and `foo.pyd`.
4. Run `uv sync` again.

Observed behavior: `foo.pyd` is not regenerated.

Expected behavior: `uv` should rebuild the project.

---

I presume this is because the workspace members are installed as editable, but the binary is only part of the wheel. `uv` nevertheless restores the `foo` package from the cache. So it ends up just symlinking to the Python code.

Note that the problem cannot be fixed by running `uv pip install -e .` either.

### Platform

multiple

### Version

0.5.28

### Python version

multiple

---

_Label `bug` added by @polw-zi on 2025-02-10 16:22_

---

_Assigned to @konstin by @charliermarsh on 2025-02-10 17:39_

---

_Comment by @polw-zi on 2025-02-10 17:52_

I just realized my instructions to reproduce the issue were not accurate and corrected them. I can correctly trigger a rebuild via `touch pyproject.toml`. A rebuild _is not_ triggered if I delete the entire virtual environment and the build artifacts.

---

_Referenced in [astral-sh/uv#11775](../../astral-sh/uv/issues/11775.md) on 2025-02-27 12:56_

---

_Unassigned @konstin by @konstin on 2025-03-04 13:40_

---

_Referenced in [astral-sh/uv#15380](../../astral-sh/uv/issues/15380.md) on 2025-08-19 13:15_

---

_Comment by @mstokes-positron on 2025-08-28 16:35_

This isn't just related to Maturin. I believe any build that generates files that are placed in the source tree run into this problem. For my use case, I have  a project using Setuptools to generate some Python files at build time. I even tried adding the generated files to `[tool.uv.cache-keys]` in the `pyproject.toml` file (hoping that a missing file would cause a cache miss), but the file was not generated when running `uv sync` again after deleting the Python file.

---

_Comment by @mmastrac on 2025-08-29 16:54_

We just ran into this with a maturin subproject as well. `touch subproject/pyproject.toml` is a workaround.

---

_Referenced in [asakatida/chimera#4715](../../asakatida/chimera/pulls/4715.md) on 2025-10-06 02:11_

---

_Comment by @fellhorn on 2025-10-22 16:08_

Another workaround is to disable uv caching for the specific package in the workspace. For maturin packages we also disable build isolation to fully leverage the cargo target dir caching.

In the pyproject.toml:
```toml
[tool.uv]
reinstall-package = ["my-maturin-package"]
no-build-isolation-package = ["my-maturin-package"]
```

If the cargo cache is up to date the .so file recreation is fairly fast and the forced reinstall won't really matter:

```sh
...
Prepared 1 package in 448ms
DEBUG Uninstalled my-maturin-package (8 files, 1 directory)
Uninstalled 1 package in 0.45ms
Installed 1 package in 0.91ms
```


---

_Comment by @konstin on 2025-10-23 10:03_

Another feature we have added since are `tool.uv.cache-keys`, which allow recompiling packages when e.g. `**/*.rs` changes.

---

_Comment by @fellhorn on 2025-10-23 11:17_

> Another feature we have added since are tool.uv.cache-keys, which allow recompiling packages when e.g. **/*.rs changes.

This did not work for one special edge case: if one deletes the `*.so` files (e.g. with `git clean -fdx`), there is no cache bust. It also did not work to add the so files to the `tool.uv.cache-keys` as the deletion apparently does not cause a cache bust.

---

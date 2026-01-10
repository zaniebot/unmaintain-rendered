---
number: 10996
title: "meson-python compiled extensions built twice with `uv sync --no-editable` and dynamic version"
type: issue
state: open
author: lgarrison
labels:
  - bug
assignees: []
created_at: 2025-01-27T18:43:46Z
updated_at: 2025-01-27T20:56:58Z
url: https://github.com/astral-sh/uv/issues/10996
synced_at: 2026-01-10T01:25:00Z
---

# meson-python compiled extensions built twice with `uv sync --no-editable` and dynamic version

---

_Issue opened by @lgarrison on 2025-01-27 18:43_

### Summary

`uv sync --no-editable` is building the compiled extensions in my project twice. The conditions to reproduce are a bit specific, it seems to require:
1. meson-python build backend,
2. `dynamic = ["version"]` in `pyproject.toml`,
3. `uv sync --no-editable`, not `uv pip install` or `pip install`.

The first compilation seems to happen in the metadata phase, under `mesonpy.build_editable`. Presumably this is related to version resolution? `build_editable` isn't called in the `uv pip` version, however. The second compilation happens in `mesonpy.build_wheel`, which seems normal.

I did a spot-check of a scikit-build-core project and didn't see a double build, for what it's worth.

While odd, this bug/behavior isn't causing any real problems, so feel free to treat it as low priority.

## Logs

[uv-sync.txt](https://github.com/user-attachments/files/18562971/uv-sync.txt)
[uv-pip.txt](https://github.com/user-attachments/files/18562972/uv-pip.txt)

The two `ccache cc ...` blocks are where the compilations are happening in the sync log.

## Steps to reproduce

Here is a reproducer on a small project. I'm happy to work on making it more minimal if that would be helpful.

```
❯ git clone https://github.com/lgarrison/pycorrfunc.git
❯ cd pycorrfunc
❯ git checkout c869af8feb936960ef3673aa0974eb332c47fc8a
❯ uv sync -v --no-editable
```


### Platform

Rocky 8 Linux

### Version

0.5.24

### Python version

3.10

---

_Label `bug` added by @lgarrison on 2025-01-27 18:43_

---

_Comment by @charliermarsh on 2025-01-27 18:45_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-27 20:56_

---

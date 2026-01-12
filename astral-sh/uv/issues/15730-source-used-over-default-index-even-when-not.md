```yaml
number: 15730
title: Source used over default index even when not specified explicitly
type: issue
state: open
author: befelix
labels:
  - bug
assignees: []
created_at: 2025-09-08T07:27:31Z
updated_at: 2025-09-08T17:57:42Z
url: https://github.com/astral-sh/uv/issues/15730
synced_at: 2026-01-12T16:02:16Z
```

# Source used over default index even when not specified explicitly

---

_@befelix_

### Summary

I'm not 100% sure this is a bug, but if not I would appreciate a hint on how to resolve the situation:

In the following `pyproject.toml` file, `torch` is both a main dependency and a dependency-group. For the dependency group, the index is pinned to the cuda wheels, while it is left unspecified otherwise. My expectation would be that, without the `cuda` group, the default index is used. However, running `uv sync` on a Mac will yield an error that a compatible release can't be found on the `cu128` mirror, even though it is available on the default pypi mirror.

`uv sync` (mac)
```
error: Distribution `torch==2.8.0+cu128 @ registry+https://download.pytorch.org/whl/cu128` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on macOS (`macosx_15_0_arm64`), but `torch` (v2.8.0+cu128) only has wheels for the following platforms: `manylinux_2_28_x86_64`, `win_amd64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

`pyproject.toml`:
```
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = ["torch"]

[dependency-groups]
cuda = ["torch>=2.5.0"]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu128", group = "cuda" },
]

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```



### Platform

macOS

### Version

uv 0.8.15 (8473ecba1 2025-09-03)

### Python version

_No response_

---

_Label `bug` added by @befelix on 2025-09-08 07:27_

---

_Comment by @jakubbortlik on 2025-09-08 13:48_

I seem to be having a similar problem: When in my `pyproject.toml` i update one of our dependencies which is taken from an index with `explicit = true` and I run `uv sync`, suddenly all of the dependencies are updated in the `uv.lock` so that instead of `source = { registry = "https://pypi.org/simple" }` they link to the internal index.

---

_Comment by @charliermarsh on 2025-09-08 13:49_

@jakubbortlik -- This sounds unrelated, can you please open a separate issue with a clear reproduction?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-08 13:49_

---

_Comment by @charliermarsh on 2025-09-08 14:58_

I also expect this to behave the way you describe, will look into it.

---

_Comment by @jakubbortlik on 2025-09-08 15:11_

> [@jakubbortlik](https://github.com/jakubbortlik) -- This sounds unrelated, can you please open a separate issue with a clear reproduction?

I'm sorry, I didn't read the issue description carefully. I'll open a separate issue if I don't find a solution. Thanks.

---

_Comment by @zanieb on 2025-09-08 17:52_

Here's the lockfile: https://gist.github.com/zanieb/e94d8db90c99b75c25dec943492cfec1#file-uv-lock
and logs: https://gist.github.com/zanieb/e94d8db90c99b75c25dec943492cfec1#file-logs

---

```yaml
number: 12480
title: "duplicate cache: uv add cannot use cache from uv pip install"
type: issue
state: open
author: xiejx5
labels:
  - question
  - cache
assignees: []
created_at: 2025-03-26T10:13:39Z
updated_at: 2025-06-12T13:56:42Z
url: https://github.com/astral-sh/uv/issues/12480
synced_at: 2026-01-12T16:01:04Z
```

# duplicate cache: uv add cannot use cache from uv pip install

---

_@xiejx5_

### Summary

Duplicate packages from uv add and uv pip commands. Minimum steps to reproduce:

```bash
uv cache clean

# first download
uv venv test
uv pip install numpy --directory test

# second download, did not use cache
uv init --bare
uv add numpy
```

### Platform

Linux 4.18.0

### Version

uv 0.6.10

### Python version

Python 3.13.2

---

_Label `bug` added by @xiejx5 on 2025-03-26 10:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-26 12:58_

---

_Comment by @charliermarsh on 2025-03-26 12:59_

I can take a look.

---

_Comment by @Aditya-PS-05 on 2025-03-26 13:20_

@charliermarsh , can I take a look at this issue and solve it?

---

_Comment by @charliermarsh on 2025-03-26 13:26_

I have an idea on what the problem is but I think it will be challenging to solve correctly. (There’s an “easy” fix that will regress a different, small bug.)

---

_Comment by @charliermarsh on 2025-03-26 15:55_

Oh sorry. This is because in `uv sync` (or `uv add`), we require hashes to be present. But in `uv pip install`, we don't. So we have to re-download the wheel to hash it.

---

_Label `bug` removed by @charliermarsh on 2025-03-26 15:55_

---

_Label `question` added by @charliermarsh on 2025-03-26 15:55_

---

_Comment by @xiejx5 on 2025-03-27 09:43_

Thank you for the answer. Is there any setting to avoid these duplicates? As uv pip can use the cache from uv add.

---

_Comment by @charliermarsh on 2025-03-27 12:59_

I think we could plausibly add `--generate-hashes` to `uv pip install` which would avoid this problem.

---

_Comment by @xiejx5 on 2025-03-27 13:11_

> I think we could plausibly add `--generate-hashes` to `uv pip install` which would avoid this problem.

Thank you. Unfortunately, `--generate-hashes` only exists in `uv pip compile` while not `uv pip install`.

Anyway, thanks a lot for your development and reply.

---

_Comment by @charliermarsh on 2025-03-27 13:12_

Ah yeah, I meant, we could add it to `uv pip install`.

---

_Label `cache` added by @zanieb on 2025-04-01 19:46_

---

_Comment by @LoSealL on 2025-06-12 10:03_

What about add from an external index? Such as torch. For instance there are two projects both requires torch==2.7.1:

```toml
# pyproject.toml
dependencies = ["torch>=2.7.1"]

[tool.uv.sources]
torch = { index = "pytorch" }
torchaudio = { index = "pytorch" }
torchvision = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

```shell
# project 1
uv add torch  # download and make a cache in `archive-v0`
```

```shell
# project 2
uv add torch  # download and make another cache in `archive-v0`
```


---

_Comment by @LoSealL on 2025-06-12 10:04_

Can I expect uv to use identical cache for same version? Because duplicated torch takes too much disk spaces

---

_Comment by @zanieb on 2025-06-12 13:40_

Packages on different registries are separated in the cache, since they're different sources.

---

_Comment by @zanieb on 2025-06-12 13:56_

We can track de-duplication in https://github.com/astral-sh/uv/issues/13995

---

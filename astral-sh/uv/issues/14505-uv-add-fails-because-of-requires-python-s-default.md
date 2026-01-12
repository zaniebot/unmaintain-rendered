```yaml
number: 14505
title: "`uv add` fails because of `requires-python`'s \"default behavior\""
type: issue
state: closed
author: trya2l
labels:
  - question
assignees: []
created_at: 2025-07-08T12:27:38Z
updated_at: 2025-07-08T14:35:52Z
url: https://github.com/astral-sh/uv/issues/14505
synced_at: 2026-01-12T16:01:50Z
```

# `uv add` fails because of `requires-python`'s "default behavior"

---

_@trya2l_

### Question

Hi, 

I'm aware of https://github.com/astral-sh/uv/issues/6780, but maybe I don't fully grasp the feedback you gave.

I'm facing a problem with dependencies resolution while trying to install darts with pytorch cpu version.

`uv init -p 3.12.11 my-project`

```txt
#requirements.txt
# i know uv add will ignore these indexes, but it's fine since i reuse the same file later on
--index-url https://pypi.org/simple
--extra-index-url https://download.pytorch.org/whl/cpu
mlflow
torch==2.7.1
u8darts[torch]==0.36

```
```txt
#.python-version
3.12.11
```

---

- Default pyproject.toml

`requires-python = ">=3.12.11"`
```
uv add -r requirements.txt --index https://download.pytorch.org/whl/cpu
Resolved 118 packages in 3.48s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.12 (`cp312`), but `markupsafe` (v3.0.2) only has wheels with the following Python implementation tag: `cp313`
```

- Modified pyproject toml

`requires-python = "==3.12.11"`
```
uv add -r requirements.txt --index https://download.pytorch.org/whl/cpu

Resolved 118 packages in 2.29s
Installed 113 packages in 425ms
 + adagio==0.2.6
 + aiohappyeyeballs==2.6.1
```

---

I do not know much about python dependency resolution, but here, I'm not sure I can do much more. For some reason, there is a conflict in the dependencies, and at least, setting the exact python version seems to resolve it.

So is this a valid use case to implement what OP asked in https://github.com/astral-sh/uv/issues/6780


### Platform

Linux 6.15.4-200.fc42.x86_64 x86_64 GNU/Linux
Podman: ghcr.io/astral-sh/uv:0.7-debian-slim

### Version

0.7.19

---

_Label `question` added by @trya2l on 2025-07-08 12:27_

---

_Renamed from "uv add fails because of `requires-python`'s "default behavior"" to "`uv add` fails because of `requires-python`'s "default behavior"" by @trya2l on 2025-07-08 12:30_

---

_Comment by @charliermarsh on 2025-07-08 12:47_

This is an issue with the PyTorch registry whereby they have an incomplete upload for `markupsafe` (they have [MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl](https://download.pytorch.org/whl/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=15ab75ef81add55874e7ab7055e9c397312385bd9ced94920f2802310c930396) but no other wheels for that version).

I think the lowest-effort fix would just be to add `markupsafe<3` as a constraint in your `requirements.txt`. Slightly better though would be to structure your `pyproject.toml` like:
```toml
[project]
name = "foo"
version = "0.1.0"
readme = "README.md"
requires-python = ">=3.12.1"
dependencies = [
    "mlflow",
    "torch==2.7.1",
    "u8darts[torch]==0.36",
]

[tool.uv.sources]
torch = { index = "pytorch-cpu" }

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

But there isn't a super-simple way to do that on the `uv add` CLI alone.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-08 12:49_

---

_Comment by @trya2l on 2025-07-08 13:14_

Fair enough, so it is more about PyTorch and my own ignorance than uv.

Thanks @charliermarsh for helping me with this. I'll stick with the requirements file as it will ease the transition to uv for my team. In any case I'm happy with this fix. 👍  :)

---

_Closed by @trya2l on 2025-07-08 13:14_

---

_Comment by @charliermarsh on 2025-07-08 14:35_

Filed on the PyTorch index at: https://github.com/pytorch/test-infra/issues/6900

---

---
number: 9087
title: Using system-level installed package on specific platforms?
type: issue
state: open
author: NGTOne
labels: []
assignees: []
created_at: 2024-11-13T15:55:31Z
updated_at: 2024-11-14T10:34:42Z
url: https://github.com/astral-sh/uv/issues/9087
synced_at: 2026-01-10T01:24:36Z
---

# Using system-level installed package on specific platforms?

---

_Issue opened by @NGTOne on 2024-11-13 15:55_

So I've got kind of an unfortunate case: I have a Docker image for `aarch64`, which contains an installed wheel for a compiled version of `tensorflow` in the system Python's `dist-packages`. `tensorflow` does not distribute a compiled wheel for `aarch64` through PyPI (or anywhere else that I can find), and I'm fairly certain that the installed version in this Docker image also isn't "standard" `tensorflow` but contains some additional customizations by Nvidia for the specific hardware platform I'm on. This means I have a very strong incentive not to pull the public wheel, since it will have an extremely lengthy build step and may not even work at all.

So the obvious answer is to specify `tensorflow` as normal, and then set a path dependency for it with an `aarch64` platform marker. However, this fails to lock, because I do not have this structure on the `amd64` system where I'm actually doing my development (and have no realistic way to generate it because of the extreme platform difference). `amd64` systems are allowed to use the standard TensorFlow distributed through PyPI.

Is there a way I can manage my case? Poetry failed with this too for similar reasons, and I ended up resorting to copying the whole package from the system Python's `dist-packages` into the venv as a Docker step.

---

_Renamed from "Using system-level installed package on specific platforms" to "Using system-level installed package on specific platforms?" by @NGTOne on 2024-11-13 15:55_

---

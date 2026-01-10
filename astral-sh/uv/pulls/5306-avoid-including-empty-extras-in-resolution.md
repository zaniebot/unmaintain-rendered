```yaml
number: 5306
title: Avoid including empty extras in resolution
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/extr
created_at: 2024-07-22T19:09:32Z
updated_at: 2024-07-22T19:57:54Z
url: https://github.com/astral-sh/uv/pull/5306
synced_at: 2026-01-10T13:37:23Z
```

# Avoid including empty extras in resolution

---

_Pull request opened by @charliermarsh on 2024-07-22 19:09_

## Summary

You can still generate instabilities, but at least it's consistent between including and excluding the extra.

For example, this resolves to 54 and then 52 packages on re-run:

```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
description = "State-of-the-art Machine Learning for JAX, PyTorch and TensorFlow"
requires-python = ">=3.9.0"

dependencies = []

[project.optional-dependencies]
flax = ["jaxlib>=0.4.1,<=0.4.13"]
onnxruntime = ["onnxruntime>=1.4.0"]
ray = ["ray[tune]>=2.7.0"]
deepspeed-testing = [
  "dill<0.3.5",
  "datasets!=2.5.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

I think the difference is just somewhere in PubGrub -- like, we add an extra dependency, so the iteration order gets changed, and we end up with a different resolution at the end.

Closes https://github.com/astral-sh/uv/issues/5285.


---

_Label `bug` added by @charliermarsh on 2024-07-22 19:10_

---

_Merged by @charliermarsh on 2024-07-22 19:57_

---

_Closed by @charliermarsh on 2024-07-22 19:57_

---

_Branch deleted on 2024-07-22 19:57_

---

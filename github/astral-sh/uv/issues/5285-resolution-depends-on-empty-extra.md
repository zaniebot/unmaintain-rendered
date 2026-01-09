---
number: 5285
title: Resolution depends on empty extra
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-22T13:30:09Z
updated_at: 2024-07-22T19:57:54Z
url: https://github.com/astral-sh/uv/issues/5285
synced_at: 2026-01-07T13:12:17-06:00
---

# Resolution depends on empty extra

---

_Issue opened by @konstin on 2024-07-22 13:30_

On linux with with uv 0.2.27, the resolution of the pyproject.toml below depends on whether `onnx = []` is present. The resolution should be robust to adding or removing empty extras, even more the resolution should not care about empty extras at all.

With `onnx`:
```console
$ rm -f uv.lock && uv lock --upgrade --preview -v 2> a.txt && wc -l uv.lock && uv lock --preview -v 2>b.txt && wc -l uv.lock
2028 uv.lock
1949 uv.lock
```

Without `onnx`:
```console
$ rm -f uv.lock && uv lock --upgrade --preview -v 2> a.txt && wc -l uv.lock && uv lock --preview -v 2>b.txt && wc -l uv.lock
1936 uv.lock
1936 uv.lock
```

```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
description = "State-of-the-art Machine Learning for JAX, PyTorch and TensorFlow"
requires-python = ">=3.9.0"

dependencies = [
  "filelock",
  "huggingface-hub>=0.19.3,<1.0",
  "numpy>=1.17",
  "packaging>=20.0",
  "pyyaml>=5.1",
  "regex!=2019.12.17",
  "requests",
  "tokenizers>=0.14,<0.19",
  "safetensors>=0.4.1",
  "tqdm>=4.27",
]

[project.optional-dependencies]
onnx = []
tf-cpu = [
  "onnxconverter-common",
]
flax = [
  "jaxlib>=0.4.1,<=0.4.13",
  "flax>=0.4.1,<=0.7.0",
  "optax>=0.0.8,<=0.1.4"
]
onnxruntime = ["onnxruntime>=1.4.0", "onnxruntime-tools>=1.4.2"]
ray = ["ray[tune]>=2.7.0"]

deepspeed-testing = [
  "dill<0.3.5",
  "datasets!=2.5.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Label `bug` added by @konstin on 2024-07-22 13:30_

---

_Label `preview` added by @konstin on 2024-07-22 13:30_

---

_Comment by @charliermarsh on 2024-07-22 13:31_

What do you mean "depends on"? Just that it varies?

---

_Comment by @konstin on 2024-07-22 13:33_

Yes, we get different lockfiles depending on whether empty extra is present or not. No idea why this is happening, i was looking for an MRE for the transformers resolution instability but than found this.

---

_Comment by @charliermarsh on 2024-07-22 13:34_

Ok, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 16:29_

---

_Referenced in [astral-sh/uv#5306](../../astral-sh/uv/pulls/5306.md) on 2024-07-22 19:09_

---

_Closed by @charliermarsh on 2024-07-22 19:57_

---

_Closed by @charliermarsh on 2024-07-22 19:57_

---

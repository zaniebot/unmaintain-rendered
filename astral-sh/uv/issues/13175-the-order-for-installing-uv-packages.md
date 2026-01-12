```yaml
number: 13175
title: The order for installing UV packages
type: issue
state: closed
author: thebarkingdog-yh
labels:
  - question
assignees: []
created_at: 2025-04-28T13:00:33Z
updated_at: 2025-04-28T19:35:19Z
url: https://github.com/astral-sh/uv/issues/13175
synced_at: 2026-01-12T16:01:21Z
```

# The order for installing UV packages

---

_@thebarkingdog-yh_

### Summary

When I have a package A that depends on `onnxruntime`, and another package B that depends on `onnxruntime-gpu`, the installation order will affect the result.

How should I resolve this issue?

---

One solution I have is: `uv pip uninstall onnxruntime onnxruntime-gpu && uv pip install onnxruntime-gpu`.

However, this results in me having to add `--no-sync` every time I run uv run, otherwise it will install onnxruntime again.

---

Another approach is to use `reinstall-package`.
```
[tool.uv]
reinstall-package = ["onnxruntime-gpu"]
```

However, it doesn’t work for the first installation (using uv sync), and if onnxruntime-gpu is reinstalled every time you run uv run, it’s not an ideal solution either.

---

Is there a better solution that allows a specific package to be installed last? Otherwise, as in the onnx example above, the installation order will affect the result.

Below is my test script. Run it to check whether the GPU is available now.

```py
import onnxruntime

available_providers: list[str] = onnxruntime.get_available_providers()

print("=== ONNX Provider Info ===")
print(f"GPU Available: {'CUDAExecutionProvider' in available_providers}")
print(f"All Available Providers: {available_providers}")
print(f"ONNX Runtime Version: {onnxruntime.__version__}")
```

If necessary, I can provide a simple pyproject.toml. However, after several tests, I found that the results are inconsistent—sometimes the final result uses the CPU, while other times it uses the GPU.


### Platform

Ubuntu 22.04

### Version

uv 0.6.17

### Python version

Python 3.13.3

---

_Label `bug` added by @thebarkingdog-yh on 2025-04-28 13:00_

---

_Comment by @konstin on 2025-04-28 13:39_

That is a tough problem: We have two packages with a dependency conflict that uv can't detect.

We could solve this by adding dependency elimination (https://github.com/astral-sh/uv/issues/4422), can you try an [override](https://docs.astral.sh/uv/reference/settings/#override-dependencies) with `onnxruntime; sys_platform == "never"` (https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800)?

---

_Label `bug` removed by @konstin on 2025-04-28 13:39_

---

_Label `question` added by @konstin on 2025-04-28 13:39_

---

_Comment by @thebarkingdog-yh on 2025-04-28 14:29_

> That is a tough problem: We have two packages with a dependency conflict that uv can't detect.
> 
> We could solve this by adding dependency elimination ([#4422](https://github.com/astral-sh/uv/issues/4422)), can you try an [override](https://docs.astral.sh/uv/reference/settings/#override-dependencies) with `onnxruntime; sys_platform == "never"` ([#4422 (comment)](https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800))?

@konstin 
I used the following TOML to create a virtual environment, but `onnxruntime` was still installed. Did I do something wrong?

```toml
[project]
name = "onnx-test"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "faster-whisper>=1.1.1",
    "onnxruntime-gpu>=1.21.1",
    "onnxruntime; sys_platform == 'never'",
]
```

---

_Comment by @konstin on 2025-04-28 15:38_

This needs to be defined as [`tool.uv.override-dependencies`](https://docs.astral.sh/uv/reference/settings/#override-dependencies) instead of `project.dependencies`.

---

_Comment by @thebarkingdog-yh on 2025-04-28 19:35_

Oh, I didn't notice it, sorry. 

I succeeded, so I'll close this issue. Thank you!

---

_Closed by @thebarkingdog-yh on 2025-04-28 19:35_

---

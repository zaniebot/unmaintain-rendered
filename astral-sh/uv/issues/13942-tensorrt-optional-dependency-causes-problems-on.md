---
number: 13942
title: TensorRT optional dependency causes problems on MacOs
type: issue
state: closed
author: PawelPeczek-Roboflow
labels:
  - question
assignees: []
created_at: 2025-06-10T11:16:07Z
updated_at: 2025-06-24T08:56:38Z
url: https://github.com/astral-sh/uv/issues/13942
synced_at: 2026-01-10T01:25:40Z
---

# TensorRT optional dependency causes problems on MacOs

---

_Issue opened by @PawelPeczek-Roboflow on 2025-06-10 11:16_

### Question

Hello team,

I've encountered issue with adding TensorRT as optional dependency to the project I intent to be multi-platform (and TRT is to be supported only where it is applicable).

```
[project]
name = "XX"
version = "0.1.0"
description = "YY"
readme = "README.md"
requires-python = ">=3.10,<3.13"
dependencies = []

[project.optional-dependencies]
trt10 = [
  "tensorrt>=10.0.0,<11.0.0; platform_system == 'Linux' or platform_system == 'Windows'",
  "tensorrt-cu12>=10.0.0,<11.0.0; platform_system == 'Linux' or platform_system == 'Windows'"
]
```

`uv sync` provides the following output:
```
❯ uv sync
  × Failed to build `tensorrt-cu12==10.11.0.33`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
[...]
      RuntimeError: TensorRT currently only builds wheels for Linux and Windows

      hint: This usually indicates a problem with the package or the build environment.
  help: `tensorrt-cu12` (v10.11.0.33) was included because `inference-exp[trt10]` (v0.1.0) depends on
        `tensorrt>=10.0.0, <11.0.0` (v10.11.0.33) which depends on `tensorrt-cu12==10.11.0.33`
```

This obviously makes sense when installing the trt on MacOS, but I am wondering if that would be possible to keep TRT dependencies enlisted and still be able to run daily development on macos

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

---

_Label `question` added by @PawelPeczek-Roboflow on 2025-06-10 11:16_

---

_Comment by @PawelPeczek-Roboflow on 2025-06-10 11:17_

I will just mention that `tensorrt>=8.0.0,<9.0.0` adds as another extras without any issues when marked with `platform_system` - seems like the auxiliary libs dependent on trt10 cause problem

---

_Comment by @charliermarsh on 2025-06-10 11:42_

Try:
```
[tool.uv]
environments = [
  "platform_system == 'Linux' or platform_system == 'Windows'",
  "platform_system != 'Linux' and platform_system != 'Windows'",
]
```

---

_Comment by @PawelPeczek-Roboflow on 2025-06-10 12:08_

that did not work, but following on this docs: https://docs.astral.sh/uv/concepts/projects/config/#build-isolation

I concluded that manually providing dependency metadata help to avoid build in situation when it's required to resolve dependencies - the downside is probably locking to specific trt version, right?

```
[[tool.uv.dependency-metadata]]
name = "tensorrt-cu12"
version = "10.11.0.33"
requires-dist = ["tensorrt-cu12-bindings", "tensorrt-cu12-libs"]

[[tool.uv.dependency-metadata]]
name = "tensorrt-cu12-libs"
version = "10.11.0.33"
requires-dist = ["nvidia-cuda-runtime-cu12"]

[[tool.uv.dependency-metadata]]
name = "nvidia-cuda-runtime-cu12"
version = "12.8.57"

[[tool.uv.dependency-metadata]]
name = "tensorrt-cu12-bindings"
version = "10.11.0.33"
```

---

_Comment by @PawelPeczek-Roboflow on 2025-06-10 12:08_

those metadata I extracted from `uv.lock` which resolved on Linux machine

---

_Comment by @charliermarsh on 2025-06-10 12:55_

I believe you can omit the `version` in those entries if you want the metadata to be applied to _all_ versions of the package.

---

_Comment by @PawelPeczek-Roboflow on 2025-06-10 12:56_

cool - thanks

---

_Closed by @PawelPeczek-Roboflow on 2025-06-24 08:56_

---

---
number: 13436
title: Error when building PyTorch from source
type: issue
state: open
author: cora-codes
labels:
  - question
assignees: []
created_at: 2025-05-13T19:28:36Z
updated_at: 2025-05-13T21:43:57Z
url: https://github.com/astral-sh/uv/issues/13436
synced_at: 2026-01-07T13:12:18-06:00
---

# Error when building PyTorch from source

---

_Issue opened by @cora-codes on 2025-05-13 19:28_

### Question

I'm using the latest version of UV.

`uv add --editable external/pytorch/`
 
Gives the following error:

```
  × Failed to build `torch @ file:///home/ubuntu/pytorch`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.prepare_metadata_for_build_editable` failed (exit
      status: 1)

      [stdout]
      -- Configuring incomplete, errors occurred!
      Building wheel torch-2.8.0a0+git20ba8fe
      -- Building version 2.8.0a0+git20ba8fe
      -- Checkout nccl release tag: v2.26.2-1
      cmake -GNinja -DBUILD_PYTHON=True -DBUILD_TEST=True -DCMAKE_BUILD_TYPE=Release
      -DCMAKE_INSTALL_PREFIX=/home/ubuntu/pytorch/torch
      -DCMAKE_PREFIX_PATH=/home/ubuntu/.cache/uv/builds-v0/.tmpiKso57/lib/python3.10/site-packages
      -DPython_EXECUTABLE=/home/ubuntu/.cache/uv/builds-v0/.tmpiKso57/bin/python
      -DTORCH_BUILD_VERSION=2.8.0a0+git20ba8fe -DUSE_NUMPY=True
      /home/ubuntu/pytorch

      [stderr]
      CMake Error at CMakeLists.txt:26 (project):
        Running

         '/home/ubuntu/.cache/uv/builds-v0/.tmpVYbIyZ/bin/ninja' '--version'

        failed with:

         no such file or directory



      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen`
        flag to skip locking and syncing.
```

Could be related to build isolation or something? I've tried cleaning the uv cache and I've ensured that ninja and cmake are installed and up to date.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @cora-codes on 2025-05-13 19:28_

---

_Comment by @cora-codes on 2025-05-13 19:44_

Just documenting other issues that have came up:

1. I needed to manually install typing extentions, pyyaml and pybind11 (`uv add typing_extensions pyyaml pybind11`). I also installed ninja and cmake, but I'm unsure if this is best practice or if apt should be used to install them instead.
2. `make clean` inside the pytorch directory fixed the error above
3. `CUDA::cuFile` can't be found as a target (I believe this is an issue with poetry too). `USE_CUFILE` can be turned off, but there isn't a way to have uv automatically set this env variable (yet)

I'm leaving the issue open since it would be nice to make this workflow less painful (by which I mean installing pytorch from source). It's very possible I am missing something and that's why I'm hitting these issues, but I can also believe most people are happy with just using the wheels and so this hasn't been done much (or `uv pip` is used instead)

---

_Comment by @konstin on 2025-05-13 20:59_

PyTorch is a complex, hard to build project with gotchas around different build systems and CUDA versions. You'll likely have a better time following https://github.com/pytorch/pytorch/blob/main/CONTRIBUTING.md than approaching this through uv, which can only use PEP 517.

---

_Comment by @cora-codes on 2025-05-13 21:23_

Yes, I am very familiar with the complexity of building PyTorch. I guess I optimistically see a future where uv can build it from source (perhaps with some patches to PyTorch which I'd be happy to submit)...

Also, flash attention works which has a similar level of complexity?

---

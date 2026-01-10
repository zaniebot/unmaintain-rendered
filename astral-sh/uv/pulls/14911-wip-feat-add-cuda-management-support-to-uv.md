```yaml
number: 14911
title: "[WIP] feat: add CUDA management support to uv"
type: pull_request
state: open
author: AlpinDale
labels: []
assignees: []
draft: true
base: main
head: uv-cuda
created_at: 2025-07-26T03:44:29Z
updated_at: 2025-09-08T13:35:15Z
url: https://github.com/astral-sh/uv/pull/14911
synced_at: 2026-01-10T06:44:33Z
```

# [WIP] feat: add CUDA management support to uv

---

_Pull request opened by @AlpinDale on 2025-07-26 03:44_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds CUDA version management and installation to uv. Users can install different CUDA versions using uv as they can python. Currently, it can be used like this:

```sh
$ uv cuda install 12.9.1
Installing CUDA 12.9.1
Extracting CUDA 12.9.1
Extraction complete
Successfully installed CUDA 12.9.1 to $HOME/.local/share/uv/cuda/cuda-12.9.1

To use this CUDA installation, run:
  uv cuda use 12.9.1
$ uv cuda use 12.9.1
Added CUDA environment to $HOME/.bashrc
CUDA 12.9.1 activated

To activate in new terminal sessions, either:
  - Restart your terminal
  - Run: source $HOME/.bashrc
$ source ~/.bashrc
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Tue_May_27_02:21:03_PDT_2025
Cuda compilation tools, release 12.9, V12.9.86
Build cuda_12.9.r12.9/compiler.36037853_0
```

This implementation design is currently a WIP, and may change based on user and maintainer feedback. Ideally, we want integration with pyproject, but right now, it's shell-based and the user would need to source their shell again, like they would after a fresh uv installation.



## Test Plan

No test units written yet. Will do after the design is approved.


---

_Comment by @alexandreteles on 2025-07-26 03:54_

Any plans to support something like `uv add cuda 12.9.1` or whatever would be the best interface for it? What about cudnn?

---

_Comment by @AlpinDale on 2025-07-26 04:09_

Yes, the interface is also WIP and I could switch to `add` if needed. I went with `install` because the python manager works like that too: `uv python install 3.12` for example.

Also good call about cudnn, I'll have to see if I can include it here. It's usually not needed by default, so maybe it can be an optional flag.

---

_Comment by @mstarodub on 2025-07-26 13:32_

It would be good to avoid all sorts of state, i.e.`add` instead of `install` (install is a misfeature of uv, imo). 
Changing the shell rc file is a very bad idea and shouldn't be part of this - just setting the env vars for `uv run` would be better.

---

_Comment by @zanieb on 2025-07-26 13:44_

Thanks for the pull request! The idea is pretty cool. Just as a heads up, we won't accept a big interface change like this without internal consensus on a design, so I don't expect this to be merged in the near term.

As a brief high-level note, as @mstarodub is saying, the main part of this design that feels weird is the `use` experience. uv is attempting to avoid non-declarative state changes like that and requiring `uv run` would probably make more sense.

---

_Comment by @alexandreteles on 2025-07-26 21:02_

@zanieb, what would be the best approach to get it merged quicker? Support it from `add`?

---

_Comment by @zanieb on 2025-07-27 03:48_

I don't think there's really a way to get it merged quickly, we are very careful about adding new features to uv as we'll need to maintain them in perpetuity. I think some consensus on what problems this solves is the first step. 

---

_Comment by @geofft on 2025-07-30 19:19_

Do you have an example use case for this beyond `nvcc --version`, e.g., is there a Python dependency you want to build from source, or are you writing CUDA code in your own package that requires `nvcc`?

I want to play around with some alternate approaches for this and I'm trying to understand what sort of things expect which environment variables. For `nvcc` interactively you want `$PATH`, but I assume some things will need `$CUDA_HOME` and other variables and I'd like to understand/document exactly what needs them and why.

(FWIW, [conda-forge/nvcc-feedstock](https://github.com/conda-forge/nvcc-feedstock/blob/main/recipe/linux/activate.sh) and [conda-forge/cuda-nvcc-feedstock](https://github.com/conda-forge/cuda-nvcc-feedstock/blob/main/recipe/activate.sh) both have activate hooks that set a whole lot of environment variables. The virtual environment ecosystem doesn't really support that, though, and as others mentioned, it's kind of distasteful. So I'd like some specific use cases to see what other options might work.)

---

_Comment by @zanieb on 2025-09-08 13:35_

We're wondering if we should be using https://pypi.org/project/nvidia-cuda-nvcc/ somehow instead

---

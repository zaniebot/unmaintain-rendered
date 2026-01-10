---
number: 14647
title: "`--torch-backend=auto` fails on systems with multiple GPUs and without `/proc/driver/nvidia/version`"
type: issue
state: closed
author: coezbek
labels:
  - bug
assignees: []
created_at: 2025-07-16T10:29:36Z
updated_at: 2025-11-02T21:21:45Z
url: https://github.com/astral-sh/uv/issues/14647
synced_at: 2026-01-10T01:25:47Z
---

# `--torch-backend=auto` fails on systems with multiple GPUs and without `/proc/driver/nvidia/version`

---

_Issue opened by @coezbek on 2025-07-16 10:29_

### Summary

This only affects systems which don't have `/sys/module/nvidia/version` or `/proc/driver/nvidia/version`, i.e. for instance on WSL2.

When running `uv pip install` with the new `--torch-backend=auto` on such a system with multiple GPUs, the detection will fallback to using `nvidia-smi`. Unfortunately, nvidia-smi will return a separate line for each graphics card which isn't considered in the code:

```
$ nvidia-smi --query-gpu=driver_version --format=csv,noheader
572.60
572.60
```

This will make `uv pip install with --torch-backend=auto` fail with the following error:

```
uv pip install -U "vllm[audio]" --torch-backend=auto
error: after parsing `572.60
`, found `572.60
`, which is not part of a valid version
```

`nvidia-smi` does not respect NVIDIA_VISIBLE_DEVICES, so there is no way from the outside to use `--torch-backend=auto` with two graphics cards at the moment.

Workaround is to run nvidia-smi, identify CUDA version there and run with `--torch-backend=cuXXX` as indicated by `nvidia-smi`.

I think the wrongly implemented line of code is the one I marked in this pullrequestreview:

https://github.com/astral-sh/uv/pull/12070#pullrequestreview-3024148877

Or in the current code base:

https://github.com/astral-sh/uv/blob/8d6d0678a71d86020caaf20107b1e81af29f471d/crates/uv-torch/src/accelerator.rs#L129

The easiest fix would be to take just the first line of the output of nvidia-smi. More elaborate would be a way to select the device to query or respecting NVIDIA_VISIBLE_DEVICE environment variable.

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.7.21

### Python version

Python 3.12.3

---

_Label `bug` added by @coezbek on 2025-07-16 10:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-16 12:52_

---

_Comment by @charliermarsh on 2025-08-22 15:51_

Sorry, I thought I fixed this!

---

_Referenced in [astral-sh/uv#15460](../../astral-sh/uv/pulls/15460.md) on 2025-08-22 17:27_

---

_Closed by @charliermarsh on 2025-11-02 21:21_

---

```yaml
number: 17623
title: Better detection for conflicting packages
type: pull_request
state: open
author: konstin
labels:
  - preview
assignees: []
base: main
head: konsti/better-conflict-detection
created_at: 2026-01-20T13:56:12Z
updated_at: 2026-01-20T13:56:12Z
url: https://github.com/astral-sh/uv/pull/17623
synced_at: 2026-01-20T14:40:58Z
```

# Better detection for conflicting packages

---

_@konstin_

In the previous iteration, conflict detection was based on top level modules. This would work if all namespace packages correctly omitted the `__init__.py`, but e.g. the nvidia packages include an empty `nvida/__init__.py`.

Instead, we track overlapping top level modules during installation and perform conflict analysis after installation, recursing only as far as necessary.

See https://github.com/astral-sh/uv/pull/13437 for a list of reported conflicts.

Before:
```
$ uv venv -c -q && uv pip install --preview nvidia-nvjitlink-cu12==12.8.93 nvidia-nvtx-cu12==12.8.90
  Resolved 2 packages in 0.99ms
  ░░░░░░░░░░░░░░░░░░░░ [0/2] Installing wheels...                                                    warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-nvjitlink-cu12` (v12.8.93).
  Installed 2 packages in 3ms
   + nvidia-nvjitlink-cu12==12.8.93
   + nvidia-nvtx-cu12==12.8.90
```

After:
```
$ uv venv -c -q && cargo run -q pip install --preview nvidia-nvjitlink-cu12==12.8.93 nvidia-nvtx-cu12==12.8.90
  Resolved 2 packages in 3ms
  Installed 2 packages in 4ms
   + nvidia-nvjitlink-cu12==12.8.93
   + nvidia-nvtx-cu12==12.8.90
```

Still detected true positive:
```
$ uv venv -c -q && cargo run -q pip install --no-progress opencv-python opencv-contrib-python --no-build --no-deps --preview
  Resolved 2 packages in 5ms
warning: The file `cv2/__init__.pyi` is provided by more than one package, which causes an install race condition and can result in a broken module. Packages containing the file:
* opencv-contrib-python (opencv_contrib_python-4.13.0.90-cp37-abi3-manylinux_2_28_x86_64.whl)
* opencv-python (opencv_python-4.13.0.90-cp37-abi3-manylinux_2_28_x86_64.whl)
Installed 2 packages in 6ms
 + opencv-contrib-python==4.13.0.90
 + opencv-python==4.13.0.90
```

---

_Review requested from @EliteTK by @konstin on 2026-01-20 13:56_

---

_Label `preview` added by @konstin on 2026-01-20 13:56_

---

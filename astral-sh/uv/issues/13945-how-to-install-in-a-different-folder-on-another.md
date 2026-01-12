```yaml
number: 13945
title: How to install in a different folder (on another drive)
type: issue
state: closed
author: thistlillo
labels:
  - question
assignees: []
created_at: 2025-06-10T13:07:14Z
updated_at: 2025-06-10T15:13:59Z
url: https://github.com/astral-sh/uv/issues/13945
synced_at: 2026-01-12T16:01:40Z
```

# How to install in a different folder (on another drive)

---

_@thistlillo_

### Question

I need to install uv on a drive other than the home folder drive (on a Linux machine).

I have looked at the install script `https://astral.sh/uv/install.sh`, but my shell knowledge is quite limited.

In this fragment I see two variables:
`_install_dir` and `UV_INSTALL_DIR`

```
# The actual path we're going to install to
    local _install_dir
    # The directory C dynamic/static libraries install to
    local _lib_install_dir
    # The install prefix we write to the receipt.
    # For organized install methods like CargoHome, which have
    # subdirectories, this is the root without `/bin`. For other
    # methods, this is the same as `_install_dir`.
    local _receipt_install_dir
    # Path to the an shell script that adds install_dir to PATH
    local _env_script_path
    # Potentially-late-bound version of install_dir to write env_script
    local _install_dir_expr
    # Potentially-late-bound version of env_script_path to write to rcfiles like $HOME/.profile
    local _env_script_path_expr
    # Forces the install to occur at this path, not the default
    local _force_install_dir
    # Which install layout to use - "flat" or "hierarchical"
    local _install_layout="unspecified"
    # A list of binaries which are shadowed in the PATH
    local _shadowed_bins=""

    # Check the newer app-specific variable before falling back
    # to the older generic one
    if [ -n "${UV_INSTALL_DIR:-}" ]; then
        _force_install_dir="$UV_INSTALL_DIR"
```

How can I launch the installation specifying as installation directory `/mnt_points/uv_install` ?



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @thistlillo on 2025-06-10 13:07_

---

_Comment by @zanieb on 2025-06-10 13:08_

See https://docs.astral.sh/uv/reference/installer/#changing-the-install-path

---

_Comment by @thistlillo on 2025-06-10 14:36_

Thank you.

The problem is that uv keeps saving cached files in the home folder, where there is no available space.

```
$-> uv pip install torch torchvision torchaudio
Using Python 3.10.15 environment at: /data-disk/workspace-user/gnn/.venv
Resolved 29 packages in 447ms
  × Failed to download `torch==2.7.1`
  ├─▶ Failed to extract archive: torch-2.7.1-cp310-cp310-manylinux_2_28_x86_64.whl
  ╰─▶ failed to write to file `/home/user/.cache/uv/.tmpJu1K2g/torch/lib/libtorch_cuda.so`: No space left on
      device (os error 28)
(gnn) user@server [/data-disk/workspace-user/gnn/src]
$-> which uv
/data-disk/.uvinstall/uv
(gnn) user@server [/
```

---

_Closed by @thistlillo on 2025-06-10 14:36_

---

_Reopened by @thistlillo on 2025-06-10 14:36_

---

_Comment by @zanieb on 2025-06-10 15:10_

I guess we're talking in https://github.com/astral-sh/uv/issues/13950 now?

---

_Closed by @zanieb on 2025-06-10 15:10_

---

_Comment by @thistlillo on 2025-06-10 15:13_

> I guess we're talking in [#13950](https://github.com/astral-sh/uv/issues/13950) now?

I thought the two topics were unrelated. My bad.

---

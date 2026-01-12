```yaml
number: 13127
title: uv pip sync fails with pylock.toml file containing multiple entries for one package
type: issue
state: closed
author: smphhh
labels:
  - bug
assignees: []
created_at: 2025-04-27T11:05:31Z
updated_at: 2025-04-27T16:07:27Z
url: https://github.com/astral-sh/uv/issues/13127
synced_at: 2026-01-12T16:01:20Z
```

# uv pip sync fails with pylock.toml file containing multiple entries for one package

---

_@smphhh_

### Summary

Given a (uv-generated) pylock.toml file that includes the following entries
```
[[packages]]
name = "torch"
version = "2.6.0"
marker = "sys_platform == 'darwin'"
index = "https://download.pytorch.org/whl/cpu"
wheels = [{ url = "https://download.pytorch.org/whl/cpu/torch-2.6.0-cp312-none-macosx_11_0_arm64.whl", hashes = { sha256 = "9a610afe216a85a8b9bc9f8365ed561535c93e804c2a317ef7fabcc5deda0989" } }]

[[packages]]
name = "torch"
version = "2.6.0+cpu"
marker = "sys_platform != 'darwin'"
index = "https://download.pytorch.org/whl/cpu"
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-linux_x86_64.whl", hashes = { sha256 = "59e78aa0c690f70734e42670036d6b541930b8eabbaa18d94e090abf14cc4d91" } },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-manylinux_2_28_aarch64.whl", hashes = { sha256 = "318290e8924353c61b125cdc8768d15208704e279e7757c113b9620740deca98" } },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-win_amd64.whl", hashes = { sha256 = "4027d982eb2781c93825ab9527f17fbbb12dbabf422298e4b954be60016f87d8" } },
]
```
the command `uv pip sync pylock.toml` fails with the error
```
error: Package `torch` must include one of: `wheels`, `directory`, `archive`, `sdist`, or `vcs`
```

After removing the entry that is not applicable to the current environment the command succeeds.

### Platform

Linux 6.6.71-0-virt x86_64 GNU/Linux

### Version

uv 0.6.17

### Python version

Python 3.12.10

---

_Label `bug` added by @smphhh on 2025-04-27 11:05_

---

_Comment by @charliermarsh on 2025-04-27 15:09_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-27 15:09_

---

_Closed by @charliermarsh on 2025-04-27 15:58_

---

_Closed by @charliermarsh on 2025-04-27 15:58_

---

_Comment by @charliermarsh on 2025-04-27 16:07_

Fixed in the next release.

---

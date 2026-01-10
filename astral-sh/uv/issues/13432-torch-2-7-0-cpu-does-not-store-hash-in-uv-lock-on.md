---
number: 13432
title: Torch 2.7.0 cpu does not store hash in  uv.lock on aarch64
type: issue
state: closed
author: 07pepa
labels:
  - bug
assignees: []
created_at: 2025-05-13T14:29:31Z
updated_at: 2025-05-13T14:47:34Z
url: https://github.com/astral-sh/uv/issues/13432
synced_at: 2026-01-10T01:25:33Z
---

# Torch 2.7.0 cpu does not store hash in  uv.lock on aarch64

---

_Issue opened by @07pepa on 2025-05-13 14:29_

### Summary

There is issue when running  `uv sync --upgrade`  it does not store aarch hash

platform sync runned on (other platform information is docker where it cause issue)
* Darwin 24.4.0 arm64
* uv 0.7.3 (Homebrew 2025-05-07)
* Python 3.12.4



here is snippet from uv.lock

```yaml
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp312-cp312-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp312-cp312-manylinux_2_28_x86_64.whl", hash = "sha256:64123c05615e27368c7a7816f6e39c6d219998693beabde0b0b9cedf91b5ed8b" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp312-cp312-win_amd64.whl", hash = "sha256:69e25c973bdd7ea24b0fa9f9792114950afaeb8f819e5723819b923f50989175" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp312-cp312-win_arm64.whl", hash = "sha256:1d7a6f33868276770a657beec7f77c7726b4da9d0739eff1b3ae64cc9a09d8e3" },
]
```


as you can see arm version for aarch64 is missing hash

this causes 
```
Failed to download `torch==2.7.0+cpu`
  ╰─▶ Hash mismatch for `torch==2.7.0+cpu` 
       Expected:
         sha256:64123c05615e27368c7a7816f6e39c6d219998693beabde0b0b9cedf91b5ed8b         
         sha256:69e25c973bdd7ea24b0fa9f9792114950afaeb8f819e5723819b923f50989175         
         sha256:1d7a6f33868276770a657beec7f77c7726b4da9d0739eff1b3ae64cc9a09d8e3 
       Computed:
         sha256:a845b6f3bda3c40f736847dede95d8bfec81fb7e11458cd25973ba13542cf1f6
         ...
```

manualy modifiing first line of snipet to ` 
{ url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp312-cp312-manylinux_2_28_aarch64.whl", hash= "sha256:a845b6f3bda3c40f736847dede95d8bfec81fb7e11458cd25973ba13542cf1f6" },` 

seems to help until  runing`uv sync --upgrade` that removes  hash

since it is only dependency without hash i assume it is a bug

### Platform

Linux 6.10.14-linuxkit aarch64 GNU/Linux

### Version

uv 0.7.3

### Python version

Python 3.12.9

---

_Label `bug` added by @07pepa on 2025-05-13 14:29_

---

_Comment by @charliermarsh on 2025-05-13 14:34_

I believe this is the same as #13318.

---

_Closed by @charliermarsh on 2025-05-13 14:34_

---

_Comment by @07pepa on 2025-05-13 14:47_

It is indeeed

---

_Referenced in [pytorch/pytorch#153469](../../pytorch/pytorch/issues/153469.md) on 2025-05-13 15:33_

---

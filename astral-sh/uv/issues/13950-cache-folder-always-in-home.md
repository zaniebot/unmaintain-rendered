---
number: 13950
title: Cache folder always in home
type: issue
state: closed
author: thistlillo
labels:
  - question
assignees: []
created_at: 2025-06-10T14:54:23Z
updated_at: 2025-07-11T02:02:16Z
url: https://github.com/astral-sh/uv/issues/13950
synced_at: 2026-01-10T01:25:40Z
---

# Cache folder always in home

---

_Issue opened by @thistlillo on 2025-06-10 14:54_

### Summary

I have installed uv in a location other than my home folder using the command:
```
$ curl -LsSf https://astral.sh/uv/install.sh | env UV_INSTALL_DIR="/data-disk/.uvinstall/" sh
```

However, uv keeps storing cached files in the home folder, where, in my case, there is no available space.

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
```

As you can see uv has been installed in `/data-disk/.uvinstall/uv`, but cached files are still in `~/.cache/uv/...`.




### Platform

Linux 5.15.0-46-generic #49-Ubuntu

### Version

0.7.12

### Python version

Python 3.10.15

---

_Label `bug` added by @thistlillo on 2025-06-10 14:54_

---

_Comment by @konstin on 2025-06-10 14:57_

`UV_INSTALL_DIR` only changes the location of the uv binaries, see https://github.com/astral-sh/uv/issues/11360. You can set the `XDG_CACHE_DIR` or `UV_CACHE_DIR` to move the cache dir specifically.

---

_Label `bug` removed by @konstin on 2025-06-10 14:57_

---

_Label `question` added by @konstin on 2025-06-10 14:57_

---

_Comment by @zanieb on 2025-06-10 14:58_

(https://docs.astral.sh/uv/reference/environment/#uv_cache_dir)

---

_Referenced in [astral-sh/uv#13945](../../astral-sh/uv/issues/13945.md) on 2025-06-10 15:10_

---

_Comment by @thistlillo on 2025-06-10 15:11_

Thank you very much. I think that the installation guide should be updated to include these details - I wasn't even aware there is a cache folder.

---

_Comment by @zanieb on 2025-06-10 15:16_

uv stores data in a variety of folders at runtime, as do most applications. Those are sort of separate from the installation location? We can update the documentation, but I don't think this should be a surprise.

---

_Comment by @thistlillo on 2025-06-11 10:09_

No, it's not a surprise at all, but I think that some more "explanations" would be much appreciated.

See also:
https://github.com/astral-sh/uv/issues/11360

linked by konstin.

Clearly, you target very complex use cases and cannot simplify too much, but ... just a bit more comments in the docs :-) 

---

_Closed by @charliermarsh on 2025-07-11 02:02_

---

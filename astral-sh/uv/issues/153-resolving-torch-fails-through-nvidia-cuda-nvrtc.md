---
number: 153
title: Resolving torch fails through nvidia-cuda-nvrtc-cu12 with 500 Internal Server Error
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-10-20T10:51:41Z
updated_at: 2023-10-23T01:07:46Z
url: https://github.com/astral-sh/uv/issues/153
synced_at: 2026-01-10T01:23:03Z
---

# Resolving torch fails through nvidia-cuda-nvrtc-cu12 with 500 Internal Server Error

---

_Issue opened by @konstin on 2023-10-20 10:51_

```
$ cargo run --bin puffin -- pip-compile -v requirements_torch.txt
    0.228815s DEBUG puffin_resolver::resolver Fetching dependencies for nvidia-cuda-nvrtc-cu12
error: HTTP status server error (500 Internal Server Error) for url (https://pypi-metadata.ruff.rs/packages/a4/05/23f8f38eec3d28e4915725b233c24d8f1a33cb6540a882f7b54be1befa02/nvidia_nccl_cu12-2.18.1-py3-none-manylinux1_x86_64.whl)
```
where the file contains just `torch`. My guess is that the file is too large (210MB)

---

_Comment by @charliermarsh on 2023-10-20 15:05_

We should get rid of our custom server and just do range requests. It's not worth maintaining the infra. Unless we can figure out how to do the lazy zip read in the Cloudflare Worker (I couldn't find a JS library for this).

---

_Comment by @charliermarsh on 2023-10-21 02:30_

Perhaps we should do both... But importantly, for now, we should add support for making range requests here.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-23 00:38_

---

_Referenced in [astral-sh/uv#168](../../astral-sh/uv/pulls/168.md) on 2023-10-23 01:05_

---

_Comment by @charliermarsh on 2023-10-23 01:07_

I ended up improving the solution to read the file lazily in the worker. The worker is kind of nice, because it gives us network caching rather than per-using caching for these (and it's a very generic solution that would even worker for other registries).

---

_Closed by @charliermarsh on 2023-10-23 01:07_

---

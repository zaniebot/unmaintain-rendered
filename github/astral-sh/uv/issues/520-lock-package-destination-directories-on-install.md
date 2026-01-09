---
number: 520
title: Lock package destination directories on install
type: issue
state: closed
author: zanieb
labels:
  - wish
assignees: []
created_at: 2023-11-30T16:12:36Z
updated_at: 2024-02-02T20:06:17Z
url: https://github.com/astral-sh/uv/issues/520
synced_at: 2026-01-07T13:12:16-06:00
---

# Lock package destination directories on install

---

_Issue opened by @zanieb on 2023-11-30 16:12_

As a follow-up to https://github.com/astral-sh/puffin/pull/516, we should have in-process locks which prevent concurrent writes to a package directory ensuring that we end up with at least one complete package e.g. when installing packages that write files to the same module. Additionally, we should ensure that linking always overrides existing files.

See https://github.com/astral-sh/puffin/pull/516#issuecomment-1834067560

---

_Added to milestone `Initial release` by @zanieb on 2023-11-30 16:13_

---

_Referenced in [astral-sh/uv#516](../../astral-sh/uv/pulls/516.md) on 2023-11-30 16:15_

---

_Label `wish` added by @charliermarsh on 2023-12-01 18:52_

---

_Comment by @konstin on 2023-12-26 15:45_

Using the `install-many` dev command with the pypi 10k most dependents dataset, it's almost certain to trigger a non-deterministic error at some point on my machine.

```
virtualenv --clear -p 3.10 .venv310 -q && VIRTUAL_ENV=.venv310 RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- install-many --no-build scripts/popular_packages/pypi_10k_most_dependents.txt
```

Both for files ...

```
puffin-dev failed
  Caused by: Failed to install
  Caused by: Failed to install: pyppeteer-1.0.2-py3-none-any.whl
  Caused by: failed to hardlink file from /home/konsti/.cache/puffin/wheels-v0/pypi/pyppeteer/pyppeteer-1.0.2-py3-none-any/README.md to /home/konsti/projects/puffin/.venv310/lib/python3.10/site-packages/README.md
```

... and directories

```
puffin-dev failed
  Caused by: Failed to install
  Caused by: Failed to install: jupyterlab-4.1.0b0-py3-none-any.whl (jupyterlab==4.1.0b0)
  Caused by: failed to create directory `/home/konsti/projects/puffin/.venv310/etc`
  Caused by: File exists (os error 17)
```


---

_Referenced in [astral-sh/uv#732](../../astral-sh/uv/pulls/732.md) on 2023-12-26 16:55_

---

_Comment by @konstin on 2024-02-02 20:06_

Fixed in #929

---

_Closed by @konstin on 2024-02-02 20:06_

---

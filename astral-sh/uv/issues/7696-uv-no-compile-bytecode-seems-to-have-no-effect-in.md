---
number: 7696
title: "uv `--no-compile-bytecode` seems to have no effect in Docker build"
type: issue
state: closed
author: akx
labels:
  - question
assignees: []
created_at: 2024-09-26T07:09:21Z
updated_at: 2024-09-26T13:52:11Z
url: https://github.com/astral-sh/uv/issues/7696
synced_at: 2026-01-10T01:24:18Z
---

# uv `--no-compile-bytecode` seems to have no effect in Docker build

---

_Issue opened by @akx on 2024-09-26 07:09_

Given a minimal Dockerfile

```
FROM ghcr.io/astral-sh/uv:0.4.16-python3.12-bookworm-slim
RUN find / -name '*.pyc' | wc -l
RUN uv pip install --system --no-compile-bytecode --no-cache-dir hai
RUN find / -name '*.pyc' | wc -l
```
and building it:

```
$ env BUILDKIT_PROGRESS=plain docker build .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 269B done
#1 DONE 0.0s

#2 [internal] load metadata for ghcr.io/astral-sh/uv:0.4.16-python3.12-bookworm-slim
#2 DONE 0.9s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [1/4] FROM ghcr.io/astral-sh/uv:0.4.16-python3.12-bookworm-slim@sha256:cdb69914e2b70a8c0c8a01af28780bfe6c1b5fad2167207b7da5f328b3f8cc0f
#4 resolve ghcr.io/astral-sh/uv:0.4.16-python3.12-bookworm-slim@sha256:cdb69914e2b70a8c0c8a01af28780bfe6c1b5fad2167207b7da5f328b3f8cc0f done
[...]
#4 DONE 1.7s

#5 [2/4] RUN find / -name '*.pyc' | wc -l
#5 0.111 0
#5 DONE 0.2s

#6 [3/4] RUN uv pip install --system --no-compile-bytecode --no-cache-dir hai
#6 0.273 Resolved 1 package in 87ms
#6 0.287 Prepared 1 package in 13ms
#6 0.288 Installed 1 package in 0.42ms
#6 0.288  + hai==0.3.0
#6 DONE 0.3s

#7 [4/4] RUN find / -name '*.pyc' | wc -l
#7 0.125 35
#7 DONE 0.1s

#8 exporting to image
#8 exporting layers 0.0s done
#8 writing image sha256:4685627209911f9fe7de6f8993524824553cc4e59c15cd1097f56cc38736a360 done
#8 DONE 0.0s
```

See steps 5 and 8 – there were zero `pyc` files in the container to begin with, after `uv pip install --system --no-compile-bytecode` there were 35.

* `--no-compile-bytecode` isn't actually documented either, but [`compile-bytecode`](https://docs.astral.sh/uv/reference/settings/#compile-bytecode) is documented to be false by default, so I shouldn't be getting `.pyc`s anyhow?
* Setting `PYTHONDONTWRITEBYTECODE=1` doesn't have an effect (not that I expected it to).


---

_Comment by @konstin on 2024-09-26 11:34_

These files are created earlier then installation and bytecode compilation, they are created during interpreter discovery: uv runs a small script to collect information on the Python interpreter, such which wheel tags it supports. This script runs with `-I` (https://github.com/astral-sh/uv/pull/2552), so it ignores `PYTHONDONTWRITEBYTECODE` and other `PYTHON*` options. This way Python bytecode compiles the parts of std that are used by the script.

---

_Label `question` added by @konstin on 2024-09-26 11:34_

---

_Comment by @akx on 2024-09-26 12:30_

Huh. That seems to imply that there is, at present, no way to actually have no `.pyc` files get created? 😕

EDIT: https://github.com/astral-sh/uv/pull/7707 :)

---

_Referenced in [astral-sh/uv#7707](../../astral-sh/uv/pulls/7707.md) on 2024-09-26 12:38_

---

_Closed by @konstin on 2024-09-26 13:52_

---

_Referenced in [astral-sh/uv#10773](../../astral-sh/uv/issues/10773.md) on 2025-01-20 08:48_

---

_Referenced in [astral-sh/uv#11178](../../astral-sh/uv/pulls/11178.md) on 2025-02-11 19:41_

---

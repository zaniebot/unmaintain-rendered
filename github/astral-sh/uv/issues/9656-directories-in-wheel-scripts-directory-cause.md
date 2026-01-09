---
number: 9656
title: Directories in wheel scripts directory cause install error
type: issue
state: closed
author: liaoya
labels:
  - compatibility
assignees: []
created_at: 2024-12-05T07:35:42Z
updated_at: 2024-12-08T05:06:15Z
url: https://github.com/astral-sh/uv/issues/9656
synced_at: 2026-01-07T13:12:18-06:00
---

# Directories in wheel scripts directory cause install error

---

_Issue opened by @liaoya on 2024-12-05 07:35_

I want to run protobuf-protoc-bin with `uvx --from protobuf-protoc-bin protoc` and get the following error

```text
❯ uvx -v --from protobuf-protoc-bin protoc --version
DEBUG uv 0.5.6
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/tshen/.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Acquired lock for `/home/tshen/.local/share/uv/tools`
DEBUG Released lock at `/home/tshen/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `/usr/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: protobuf-protoc-bin*
DEBUG Found fresh response for: https://pypi.org/simple/protobuf-protoc-bin/
DEBUG Searching for a compatible version of protobuf-protoc-bin (*)
DEBUG Selecting: protobuf-protoc-bin==29.0 [compatible] (protobuf_protoc_bin-29.0-py2.py3-none-manylinux_2_24_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/23/2f841df570653ae9fee3057c9fef6a24c8deef756fafe7416d005464fd69/protobuf_protoc_bin-29.0-py2.py3-none-manylinux_2_24_x86_64.whl.metadata
DEBUG Tried 1 versions: protobuf-protoc-bin 1
DEBUG marker environment resolution took 0.000s
Resolved 1 package in 0.95ms
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: protobuf-protoc-bin==29.0
error: Failed to install: protobuf_protoc_bin-29.0-py2.py3-none-manylinux_2_24_x86_64.whl (protobuf-protoc-bin==29.0)
  Caused by: The wheel is invalid: Wheel contains an invalid entry (directory) in the `scripts` directory: /home/tshen/.cache/uv/builds-v0/.tmpv4pFTz/lib/python3.12/site-packages/protobuf_protoc_bin-29.0.data/scripts/include
```

I also try `uv pip install protobuf-protoc-bin` and get the following error

```text
❯ uv pip install protobuf-protoc-bin
Resolved 1 package in 0.98ms
error: Failed to install: protobuf_protoc_bin-29.0-py2.py3-none-manylinux_2_24_x86_64.whl (protobuf-protoc-bin==29.0)
  Caused by: The wheel is invalid: Wheel contains an invalid entry (directory) in the `scripts` directory: /work/cdc-stash/exost/exos-proxy-py/.venv/lib/python3.12/site-packages/protobuf_protoc_bin-29.0.data/scripts/include
```

I can do this with `pip`

```text
❯ .venv/bin/pip -v install protobuf-protoc-bin
Using pip 24.3.1 from /work/cdc-stash/exost/exos-proxy-py/.venv/lib/python3.12/site-packages/pip (python 3.12)
Looking in indexes: https://mirrors.aliyun.com/pypi/simple/
Collecting protobuf-protoc-bin
  Downloading https://mirrors.aliyun.com/pypi/packages/d0/23/2f841df570653ae9fee3057c9fef6a24c8deef756fafe7416d005464fd69/protobuf_protoc_bin-29.0-py2.py3-none-manylinux_2_24_x86_64.whl (3.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.3/3.3 MB 1.1 MB/s eta 0:00:00
Installing collected packages: protobuf-protoc-bin
Successfully installed protobuf-protoc-bin-29.0

❯ .venv/bin/protoc --version
libprotoc 29.0
```

---

_Label `compatibility` added by @konstin on 2024-12-05 08:48_

---

_Comment by @konstin on 2024-12-05 08:52_

Python wheels have a `<name>-<version>.data/scripts` directory, which allows copying binaries to a directory in the virtualenv environment that will be added to PATH when using the virtual environment. In this case, there's a `protoc` binary in the wheel. But in this wheel, there's also a directory in the `scripts` folder. The spec says to replace the python interpreter path at the beginning of scripts if any, but it's ambiguous about directories.

---

_Renamed from "uv 0.5.6 cannot install or run protobuf-protoc-bin" to "Directories in wheel scripts directoy cause install error" by @konstin on 2024-12-05 08:53_

---

_Renamed from "Directories in wheel scripts directoy cause install error" to "Directories in wheel scripts directory cause install error" by @konstin on 2024-12-05 08:53_

---

_Comment by @charliermarsh on 2024-12-06 00:04_

I guess my read is that we should move anything in `scripts` over to the environment's `scripts` directory recursively, but that we should only rewrite shebangs at the top-level...? Lemme check pip.

---

_Comment by @liaoya on 2024-12-06 00:34_

Thank you! 
I will submit a ticket to [protobuf-protoc-bin](https://github.com/RobertoRoos/protobuf-protoc-bin) if this is their issue.

---

_Comment by @charliermarsh on 2024-12-06 00:36_

It's seems like a strange behavior. I think those files should be in the `data` subdirectory instead of `scripts`? Though we may also want to change our behavior here... I'm not sure. We're clearly "stricter" than pip, but we may also be "wrong".

---

_Comment by @charliermarsh on 2024-12-06 00:39_

From reading the code, I _think_ pip moves the entire subdirectory and _does_ rewrite shebangs on nested Python files.

---

_Comment by @charliermarsh on 2024-12-06 00:46_

I chatted a bit with a pip maintainer in Discord, and the feeling is that this is indeed a mistake and the files should instead be in the `data` subdirectory. They expressed that ideally pip would flag this as an error too, though that would take more time to change since it would be breaking. So, I think we could go either way here: we could decide to mirror pip and allow this, or we could stick with our error.

---

_Comment by @liaoya on 2024-12-06 05:51_

@charliermarsh Thank you. So [protobuf-protoc-bin](https://github.com/RobertoRoos/protobuf-protoc-bin) does not follow the pip rule strictly. I think uv can choose any decision.

---

_Referenced in [pypa/packaging.python.org#1673](../../pypa/packaging.python.org/pulls/1673.md) on 2024-12-06 09:32_

---

_Comment by @charliermarsh on 2024-12-08 05:06_

Ok, it looks like this is now “officially” an issue with the package, and I think we should keep our current behavior: https://github.com/pypa/packaging.python.org/pull/1673#issuecomment-2525415109. Thanks for reporting!

---

_Closed by @charliermarsh on 2024-12-08 05:06_

---

_Referenced in [RobertoRoos/protobuf-protoc-bin#16](../../RobertoRoos/protobuf-protoc-bin/issues/16.md) on 2024-12-09 02:40_

---

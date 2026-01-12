```yaml
number: 12711
title: uv pip install hangs when I install paddlepaddle
type: issue
state: closed
author: HydrogenSulfate
labels:
  - bug
assignees: []
created_at: 2025-04-07T09:09:26Z
updated_at: 2025-07-23T08:23:22Z
url: https://github.com/astral-sh/uv/issues/12711
synced_at: 2026-01-12T16:01:11Z
```

# uv pip install hangs when I install paddlepaddle

---

_@HydrogenSulfate_

### Summary

When I use `pip install ...` in colab, `paddlepaddle` can be installed successfully, but hangs if I changed to `uv pip install ...`

![Image](https://github.com/user-attachments/assets/02e6bf72-fa47-4f57-9d8e-aca11a65c044)

### Platform

Ubuntu

### Version

uv 0.5.29

### Python version

Python 3.11.11

Related PR: <https://github.com/deepmodeling/deepmd-kit/pull/4694>

---

_Label `bug` added by @HydrogenSulfate on 2025-04-07 09:09_

---

_Comment by @njzjz on 2025-04-07 14:22_

Detailed log:

```sh
uv pip install -vvv --system "paddlepaddle-gpu==3.0.0" -i https://www.paddlepaddle.org.cn/packages/stable/cu126/
```

```
DEBUG uv 0.6.12
DEBUG Searching for default Python interpreter in search path
TRACE Searching PATH for executables: python, python3
TRACE Checking `PATH` directory for interpreters: /usr/local/cuda/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Querying interpreter executable at /usr/local/bin/python
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/usr/local/bin/python` (first executable in the search path)
Using Python 3.11.11 environment at: /usr
TRACE Checking lock for `/usr` at `/tmp/uv-b17927aca3d28934.lock`
DEBUG Acquired lock for `/usr`
DEBUG At least one requirement is not satisfied: paddlepaddle-gpu==3.0.0
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11.11
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: paddlepaddle-gpu>=3.0.0, <3.0.0+
TRACE Fetching metadata for paddlepaddle-gpu from https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies    
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: paddlepaddle-gpu. remaining choices: 
TRACE No cache entry exists for /root/.cache/uv/simple-v15/index/39f4eb9d54e2813f/paddlepaddle-gpu.rkyv
DEBUG No cache entry for: https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
TRACE Sending fresh GET request for https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
TRACE Handling request for https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/ with authentication policy auto
TRACE Request for https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
TRACE Attempting unauthenticated request for https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
TRACE checkout waiting for idle connection: ("https", [www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/))
DEBUG starting new connection: https://www.paddlepaddle.org.cn/    
TRACE Http::connect; scheme=Some("https"), host=Some("[www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/)"), port=None
DEBUG connecting to 106.12.155.111:443
DEBUG connected to 106.12.155.111:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", [www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/))
TRACE Cached request https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/ is storable because its response has a heuristically cacheable status code 200
TRACE put; add idle connection for ("https", [www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/))
DEBUG pooling idle connection for ("https", [www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/))
TRACE Received package metadata for: paddlepaddle-gpu
TRACE Selecting candidate for paddlepaddle-gpu with range >=3.0.0, <3.0.0+ with 2 remote versions
DEBUG Searching for a compatible version of paddlepaddle-gpu (>=3.0.0, <3.0.0+)
TRACE Selecting candidate for paddlepaddle-gpu with range >=3.0.0, <3.0.0+ with 2 remote versions
TRACE Found candidate for package paddlepaddle-gpu with range >=3.0.0, <3.0.0+ after 1 steps: 3.0.0 version
TRACE Returning candidate for package paddlepaddle-gpu with range >=3.0.0, <3.0.0+ after 1 steps
TRACE Found candidate for package paddlepaddle-gpu with range >=3.0.0, <3.0.0+ after 1 steps: 3.0.0 version
TRACE Returning candidate for package paddlepaddle-gpu with range >=3.0.0, <3.0.0+ after 1 steps
DEBUG Selecting: paddlepaddle-gpu==3.0.0 [compatible] (paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl)
TRACE idle interval checking for expired
TRACE No cache entry exists for /root/.cache/uv/wheels-v5/index/39f4eb9d54e2813f/paddlepaddle/3.0.0-cp311-cp311-linux_x86_64.msgpack
DEBUG No cache entry for: https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Sending fresh HEAD request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Handling request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl with authentication policy auto
TRACE Request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Attempting unauthenticated request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE checkout waiting for idle connection: ("https", paddle-whl.bj.bcebos.com)
DEBUG starting new connection: https://paddle-whl.bj.bcebos.com/    
TRACE Http::connect; scheme=Some("https"), host=Some("paddle-whl.bj.bcebos.com"), port=None
DEBUG connecting to 103.235.47.176:443
DEBUG connected to 103.235.47.176:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", paddle-whl.bj.bcebos.com)
TRACE put; add idle connection for ("https", paddle-whl.bj.bcebos.com)
DEBUG pooling idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE Cached request https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is storable because its response has an 'Expires' header set
TRACE Getting metadata for paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl by range request
TRACE Handling request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl with authentication policy auto
TRACE Request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Attempting unauthenticated request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE take? ("https", paddle-whl.bj.bcebos.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE put; add idle connection for ("https", paddle-whl.bj.bcebos.com)
DEBUG pooling idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE Handling request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl with authentication policy auto
TRACE Request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Attempting unauthenticated request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE take? ("https", paddle-whl.bj.bcebos.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE put; add idle connection for ("https", paddle-whl.bj.bcebos.com)
DEBUG pooling idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE Handling request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl with authentication policy auto
TRACE Request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Attempting unauthenticated request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE take? ("https", paddle-whl.bj.bcebos.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE put; add idle connection for ("https", paddle-whl.bj.bcebos.com)
DEBUG pooling idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE Handling request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl with authentication policy auto
TRACE Request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE Attempting unauthenticated request for https://paddle-whl.bj.bcebos.com/stable/cu126/paddlepaddle-gpu/paddlepaddle-3.0.0-cp311-cp311-linux_x86_64.whl
TRACE take? ("https", paddle-whl.bj.bcebos.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE put; add idle connection for ("https", paddle-whl.bj.bcebos.com)
DEBUG pooling idle connection for ("https", paddle-whl.bj.bcebos.com)
TRACE Received built distribution metadata for: paddlepaddle==3.0.0
TRACE idle interval checking for expired
TRACE idle interval evicting closed for ("https", [www.paddlepaddle.org.cn](http://www.paddlepaddle.org.cn/))
TRACE idle interval evicting closed for ("https", paddle-whl.bj.bcebos.com)
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
TRACE idle interval checking for expired
```

---

_Comment by @eric-gitta-moore on 2025-05-02 14:58_

same here.

At the same time, I encountered another issue. If the package name (paddlepaddle-gpu) and the name of the whl file (paddlepaddle-3.0.0-cp312-cp312-linux_x86_64.whl) are inconsistent, it will lead to the failure of installing `uv sync`.

project.toml
```toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "paddlepaddle-gpu==3.0.0",
]

[tool.uv.pip]
index-strategy = "unsafe-best-match"

[tool.uv.sources]
paddlepaddle-gpu = [{ index = "paddlepaddle" }]

[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
default = true

[[tool.uv.index]]
name = "paddlepaddle"
url = "https://www.paddlepaddle.org.cn/packages/stable/cu126/"
explicit = true
```

```diff
❯ uv sync -v
DEBUG uv 0.7.2
DEBUG Found project root: `/home/linux/workspace/uv-test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/linux/workspace/uv-test`
DEBUG Reading Python requests from version file at `/home/linux/workspace/uv-test/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies the request: `Python 3.12`
DEBUG Released lock at `/tmp/uv-f041222ac8a8b3f6.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test @ file:///home/linux/workspace/uv-test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: uv-test*
DEBUG Searching for a compatible version of uv-test @ file:///home/linux/workspace/uv-test (*)
DEBUG Adding direct dependency: paddlepaddle-gpu>=3.0.0, <3.0.0+
DEBUG Found stale response for: https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
DEBUG Sending revalidation request for: https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
DEBUG Found modified response for: https://www.paddlepaddle.org.cn/packages/stable/cu126/paddlepaddle-gpu/
DEBUG Searching for a compatible version of paddlepaddle-gpu (>=3.0.0, <3.0.0+)
+DEBUG Selecting: paddlepaddle-gpu==3.0.0 [compatible] (paddlepaddle-3.0.0-cp312-cp312-linux_x86_64.whl)
+error: The index returned metadata for the wrong package: expected distribution for paddlepaddle-gpu, got distribution for paddlepaddle
```

---

_Comment by @eric-gitta-moore on 2025-05-02 15:06_

update 2

Actually, I found that there is a corresponding whl file (paddlepaddle_gpu-3.0.0-cp312-cp312-linux_x86_64.whl), but perhaps due to some reason, it was not used? (I guess it's because paddlepaddle-3.0.0-cp312-cp312-linux_x86_64.whl is listed earlier in the sequence, so I chose the first one.)

![Image](https://github.com/user-attachments/assets/b2c462af-38c1-4765-8225-998e1fffc12d)

Now I have solved this problem by using a method which doesn't feel very good to me.

```diff
[project]
name = "lpd-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
-    "paddlepaddle-gpu==3.0.0",
+    "paddlepaddle-gpu==3.0.0.dev20250324",
]
```

---

_Comment by @charliermarsh on 2025-05-02 15:09_

We should probably ignore wheels entirely with the wrong name. (We should _definitely_ not allow them to be installed, but in that case, I agree it should use the `paddlepaddle-gpu` wheels.)

---

_Comment by @notatallshaw on 2025-05-02 15:23_

FYI, I'm going to make an issue on pip side as well, pip should also ignore wheels with the wrong name. Can't be done any earlier than pip 25.3 though, as we're still in the middle of deprecating invalid wheel file names.

---

_Comment by @konstin on 2025-07-02 16:25_

We don't allow wheels with the wrong name anymore and we fixed that hang, so this issue should be fixed. (Feel free to reopen it if we missed something)

---

_Closed by @konstin on 2025-07-02 16:25_

---

```yaml
number: 15713
title: "\"uv pip install\" tries to download already installed package"
type: issue
state: closed
author: vbooka1
labels:
  - question
assignees: []
created_at: 2025-09-07T17:14:44Z
updated_at: 2025-09-09T07:43:24Z
url: https://github.com/astral-sh/uv/issues/15713
synced_at: 2026-01-12T16:02:15Z
```

# "uv pip install" tries to download already installed package

---

_@vbooka1_

### Summary

Hello, I want to install package `sglang`, I have manually downloaded and installed `torch` and all other requirements like `nvidia-*` into  a `venv` called "sglang" as you can see below, but when I try to run `uv pip instal "sglang[all]>=0.5.2rc2"` somewhy `uv` tries to download all requirements again, for example `torch`:

```
(sglang) user@localhost:~/sglang$ ./bin/python  -m uv  pip install --extra-index-url file:///path/to/my/local/repo/ "sglang[all]>=0.5.2rc2"
Using Python 3.13.3 environment at: 
Resolved 150 packages in 82ms
  × Failed to download and build `flashinfer-python==0.3.0`
  ├─▶ Failed to install requirements from `build-system.requires`
  ├─▶ Failed to download `torch==2.8.0`
  ├─▶ Failed to fetch:
  │   `https://files.pythonhosted.org/packages/16/82/3948e54c01b2109238357c6f86242e6ecbf0c63a1af46906772902f82057/torch-2.8.0-cp313-cp313-manylinux_2_28_x86_64.whl`
  ├─▶ Request failed after 3 retries
  ├─▶ error sending request for url
  │   (https://files.pythonhosted.org/packages/16/82/3948e54c01b2109238357c6f86242e6ecbf0c63a1af46906772902f82057/torch-2.8.0-cp313-cp313-manylinux_2_28_x86_64.whl)
  ├─▶ client error (Connect)
  ├─▶ dns error
  ╰─▶ failed to lookup address information: Temporary failure in name resolution
  help: `flashinfer-python` (v0.3.0) was included because `sglang[all]` (v0.5.2rc2) depends on
        `flashinfer-python`
```

the `torch` is indeed installed:

```
(sglang) user@localhost:~/sglang$ ls -lh ./lib/python3.13/site-packages/torch-2.8.0.dist-info/
total 1.3M
-rw-rw-r-- 1 user user  115 Sep  7 11:33 direct_url.json
-rw-rw-r-- 2 user user  199 Sep  7 11:33 entry_points.txt
-rw-rw-r-- 1 user user    2 Sep  7 11:33 INSTALLER
drwxrwxr-x 2 user user 4.0K Sep  7 11:33 licenses
-rw-rw-r-- 2 user user  31K Sep  7 11:33 METADATA
-rw-rw-r-- 1 user user 1.2M Sep  7 11:33 RECORD
-rw-rw-r-- 1 user user    0 Sep  7 11:33 REQUESTED
-rw-rw-r-- 2 user user   25 Sep  7 11:33 top_level.txt
-rw-rw-r-- 1 user user  127 Sep  7 11:33 uv_cache.json
-rw-rw-r-- 2 user user  113 Sep  7 11:33 WHEEL
(sglang) user@localhost:~/sglang$ du -sh ./lib/python3.13/site-packages/torch
1.7G    ./lib/python3.13/site-packages/torch
```

it is absolutely undoubtedly really truly installed 

```
(sglang) user@localhost:~/sglang$ ./bin/python  -m uv  -vv pip install --extra-index-url file:///path/to/my/local/repo/ torch==2.8.0
DEBUG uv 0.8.15
DEBUG Searching for default Python interpreter in virtual environments
TRACE Found cached interpreter info for Python 3.13.3, skipping query of: bin/python
DEBUG Found `cpython-3.13.3-linux-x86_64-gnu` at `/home/user/sglang/bin/python` (parent interpre
ter)
Using Python 3.13.3 environment at: 
TRACE Checking lock for `` at `.lock`
DEBUG Acquired lock for ``
...
...
DEBUG Requirement satisfied: filelock
DEBUG Requirement satisfied: fsspec
DEBUG Requirement satisfied: jinja2
DEBUG Requirement satisfied: markupsafe>=2.0
DEBUG Requirement satisfied: mpmath>=1.1.0, <1.4
DEBUG Requirement satisfied: networkx
DEBUG Requirement satisfied: nvidia-cublas-cu12
DEBUG Requirement satisfied: nvidia-cublas-cu12==12.8.4.1 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cuda-cupti-cu12==12.8.90 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cuda-nvrtc-cu12==12.8.93 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cuda-runtime-cu12==12.8.90 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cudnn-cu12==9.10.2.21 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cufft-cu12==11.3.3.83 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cufile-cu12==1.13.1.3 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-curand-cu12==10.3.9.90 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cusolver-cu12==11.7.3.90 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cusparse-cu12
DEBUG Requirement satisfied: nvidia-cusparse-cu12==12.5.8.93 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-cusparselt-cu12==0.7.1 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-nccl-cu12==2.27.3 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-nvjitlink-cu12
DEBUG Requirement satisfied: nvidia-nvjitlink-cu12==12.8.93 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: nvidia-nvtx-cu12==12.8.90 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: setuptools ; python_full_version >= '3.12'
DEBUG Requirement satisfied: setuptools>=40.8.0
DEBUG Requirement satisfied: sympy>=1.13.3  
DEBUG Requirement satisfied: torch==2.8.0   
DEBUG Requirement satisfied: triton==3.4.0 ; platform_machine == 'x86_64' and sys_platform == 'linux'
DEBUG Requirement satisfied: typing-extensions>=4.10.0
Audited 1 package in 6ms
DEBUG Released lock at `/home/user/sglang/.lock`
```

as well as `nvidia-*`, but `uv pip install sglang` still wants to download and reinstall the very same versions of all requirements I've installed manually. 

### Platform

Debian 12

### Version

uv 0.8.15

### Python version

Python 3.13.3

---

_Label `bug` added by @vbooka1 on 2025-09-07 17:14_

---

_Comment by @zanieb on 2025-09-07 17:19_

The failure log has

```
  × Failed to download and build `flashinfer-python==0.3.0`
  ├─▶ Failed to install requirements from `build-system.requires`
  ├─▶ Failed to download `torch==2.8.0`
```

Notice we're failing to download torch while building `flashinfer-python`. This torch is going to be installed into an isolated build environment per PEP 517, not your environment where it's already installed.

---

_Label `bug` removed by @zanieb on 2025-09-08 13:25_

---

_Label `question` added by @zanieb on 2025-09-08 13:25_

---

_Comment by @vbooka1 on 2025-09-08 17:43_

Why `uv` ignores `--extra-index-url`? I have all requirements downloaded in advance to speed up the installs and reinstalls. Is there any way to make `uv` prefer local directory instead of a network mirror?

Well I see that `pip` acts the same, so it is not an `uv` bug but `pip` design flaw. How do I make `pip` or `uv pip` install packages offline, using a local directory with all requrements?

---

_Comment by @zanieb on 2025-09-08 17:49_

Maybe you're looking for `--find-links`?

---

_Comment by @vbooka1 on 2025-09-08 18:21_

I've edited the message before refreshing the page so I did not see your answer.

> Maybe you're looking for `--find-links`?

unfortunately this does not work too, both `uv pip` and plain `pip` ignore `--extra-index-url ...` and `--find-links ...` and `--extra-index-url ... --find-links ...` together, and try to download requirements from the Internet. Please tell how I could install packages with `pip` or `uv pip` on an offline machine.

---

_Comment by @zanieb on 2025-09-08 18:35_

You can use `--offline` to prevent uv from searching remote sources. I'm not sure why your local ones are not being picked up without more details.

---

_Comment by @vbooka1 on 2025-09-09 07:43_

Thank you, adding `--offline` helped. 

So the underlying problem is:

> Notice we're failing to download torch while building flashinfer-python. This torch is going to be installed into an isolated build environment per PEP 517, not your environment where it's already installed.

- `uv` and `pip` re-download all dependencies when they are building sub-dependencies, however in the `--offline` mode they do prefer local wheels.

For the future reference: an offline build of `sglang` is something like this:

``` 
## first copy the ~/.cargo/ directory with all "crates" from a connected machine to the offline machine.
cd /path/to/venv/
source ./bin/activate
CARGO_NET_OFFLINE=true ./bin/python -m uv -v pip install --offline --find-links file:///path/to/directory/with/whl/files/  "sglang[all]>=0.5.2rc2"
```


---

_Closed by @vbooka1 on 2025-09-09 07:43_

---

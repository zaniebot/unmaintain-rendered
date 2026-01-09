---
number: 8832
title: uv unable to download python due to corp proxy certificate
type: issue
state: closed
author: bouianpe
labels:
  - question
assignees: []
created_at: 2024-11-05T17:08:13Z
updated_at: 2024-11-05T17:23:54Z
url: https://github.com/astral-sh/uv/issues/8832
synced_at: 2026-01-07T13:12:18-06:00
---

# uv unable to download python due to corp proxy certificate

---

_Issue opened by @bouianpe on 2024-11-05 17:08_

Hi, I've looking into switching over from rye to uv for some of our projects for a bit, and I think the only thing stopping us now is that uv fails to install python in our corporate environment due to corporate proxy certificates. Rye is able to install python without those same issues, so it is able to find / load those certificates somewhere.
There are no issues adding / syncing / locking dependencies, both from PyPI and private Gitlab package repositories.

rye
> $ rye toolchain fetch cpython@3.11.9
Downloading cpython@3.11.9
Checking checksum
Unpacking
Downloaded cpython@3.11.9

uv
> $ uv python install -v cpython-3.11.10
**DEBUG uv 0.4.30 (61ed2a236 2024-11-04)**
DEBUG Acquired lock for `C:\Users\bouianpe\AppData\Roaming\uv\python`
DEBUG No installation found for request `cpython-3.11.10-any-any-any`
DEBUG Found download `cpython-3.11.10-windows-x86_64-none` for request `cpython-3.11.10-any-any-any`
DEBUG Using request timeout of 30s
DEBUG Transient request failure for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Transient request failure for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
error: Failed to install cpython-3.11.10-windows-x86_64-none
  Caused by: Failed to download https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
DEBUG Released lock at `C:\Users\bouianpe\AppData\Roaming\uv\python\.lock`



---

_Comment by @zanieb on 2024-11-05 17:20_

Have you used the `--native-tls` flag?

---

_Label `question` added by @zanieb on 2024-11-05 17:20_

---

_Comment by @bouianpe on 2024-11-05 17:23_

Well, that did the trick, _much_ appreciated. Looks like I need to look over the docs some more for other things I may have missed.

---

_Closed by @bouianpe on 2024-11-05 17:23_

---

_Comment by @zanieb on 2024-11-05 17:23_

No problem! I'd recommend putting that in a global configuration file if you're using corporate proxies.

---

_Referenced in [astral-sh/uv#8161](../../astral-sh/uv/issues/8161.md) on 2024-11-07 10:27_

---

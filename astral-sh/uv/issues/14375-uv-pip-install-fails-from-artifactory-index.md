---
number: 14375
title: "\"uv pip install\" fails from Artifactory index"
type: issue
state: closed
author: geokarak
labels:
  - bug
assignees: []
created_at: 2025-06-30T18:57:29Z
updated_at: 2025-07-04T02:07:10Z
url: https://github.com/astral-sh/uv/issues/14375
synced_at: 2026-01-10T01:25:44Z
---

# "uv pip install" fails from Artifactory index

---

_Issue opened by @geokarak on 2025-06-30 18:57_

### Summary

Recent versions of uv (such as the current latest v0.7.17) fail to install packages from a private Artifactory index, whereas older versions (such as v0.1.9) work.

### Reproduce error

```bash
python3 -m venv .venv
source ./.venv/bin/activate

# update pip to v25.0.1
pip install --upgrade pip

# install uv latest version (v0.7.17) (*)
pip install uv

# set uv index and cert env vars
export UV_INDEX_URL=https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/simple
export UV_FIND_LINKS=https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi
export SSL_CERT_FILE=/xxx/yyy/zzz/pypi.pem

uv pip install tqdm
```

FYI: The reason I have to specify `SSL_CERT_FILE` is that I'm running behing a corporate proxy.

Running `uv pip install tqdm` (or any other library) will produce the following error stack trace:

```bash
...
error: Failed to read `--find-links` URL: https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/
  Caused by: Failed to fetch: `https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/`
  Caused by: HTTP status client error (406 Not Acceptable) for url (https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/)
```

The above steps work as expected if one uses an older version of uv,  such as v0.1.9 - that is, substituing (*) with `uv pip install uv==0.1.9` allows the setup to run without any issues. I haven’t determined which exact version ranges of uv work properly and which ones exhibit the problem. It's likely that the problem extends beyond the v0.7.x versions of uv.

Finally, while I first noted the issue on an RHEL 7.9 system with Python 3.8.18, I also tested it on a Windows machine running Python 3.11.4 and encountered the same exact error stack trace.






### Platform

RHEL v7.9

### Version

uv 0.7.17

### Python version

Python 3.8.18

---

_Label `bug` added by @geokarak on 2025-06-30 18:57_

---

_Assigned to @jtfmumm by @zanieb on 2025-06-30 19:28_

---

_Comment by @jtfmumm on 2025-07-01 12:52_

Do you encounter this problem with uv `0.7.15`?

---

_Comment by @zanieb on 2025-07-01 12:54_

Why are you using `UV_FIND_LINKS=https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/simple`? Do you mean to just use `UV_INDEX_URL=https://artifactory.xxx.yyy.com:port/artifactory/api/pypi/eoc-pypi/simple`? It's quite unusual to set both of those to a similar target.

---

_Comment by @geokarak on 2025-07-01 15:32_

@jtfmumm @zanieb I have unset/removed the `UV_FIND_LINKS ` env var from my venv setup script, only keeping `UV_INDEX_URL`, and I confirm this is now working perfectly with newer versions of uv.

I'm not sure why/how this is working for older versions of uv (such as 0.1.9) where both env vars are set. And I also don't fully understand how the `UV_FIND_LINKS` is badly interfering with `UV_INDEX_URL`when both are set.

In any case, thanks for the help and for looking into my issue so promptly. Much appreciated!

---

_Comment by @zanieb on 2025-07-01 15:59_

There's a new regression with `UV_FIND_LINKS`, see https://github.com/astral-sh/uv/issues/14367

Glad it's working though!

---

_Closed by @charliermarsh on 2025-07-04 02:07_

---

```yaml
number: 14187
title: Which environment variable should be adjusted if venv cannot be created on the Chinese network
type: issue
state: closed
author: Ryden-kai
labels:
  - question
assignees: []
created_at: 2025-06-21T13:00:02Z
updated_at: 2025-07-04T02:08:48Z
url: https://github.com/astral-sh/uv/issues/14187
synced_at: 2026-01-10T03:32:45Z
```

# Which environment variable should be adjusted if venv cannot be created on the Chinese network

---

_Issue opened by @Ryden-kai on 2025-06-21 13:00_

### Question

CPython cannot be downloaded, which environment variable should be adjusted? How can CPython be installed normally besides using a proxy?
Here are the error details for creating venv:
root@e89b3fcd9120:~/trainer_app# uv venv -p 3.11
  × Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250612/cpython-3.11.13%2B20250612-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  ├─▶ Request failed after 3 retries
  ├─▶ error sending request for url
  │   (https://github.com/astral-sh/python-build-standalone/releases/download/20250612/cpython-3.11.13%2B20250612-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  ╰─▶ operation timed out

### Platform

Ubuntu

### Version

0.7.13

---

_Label `question` added by @Ryden-kai on 2025-06-21 13:00_

---

_Comment by @FishAlchemist on 2025-06-21 14:07_

I guess your network environment cannot access Github.com normally, right?
You're not using a proxy to access GitHub, your only option is to look for mirror sources.

https://docs.astral.sh/uv/reference/environment/#uv_python_install_mirror
https://docs.astral.sh/uv/reference/environment/#uv_pypy_install_mirror

You can use this, though I'm not sure how.
https://docs.astral.sh/uv/reference/environment/#uv_python_downloads_json_url
I'd suggest you look for sources about mirroring python-build-standalone in Chinese communities.
(I've removed the suggested mirror sources; I'm afraid I might have gotten them wrong.)


---

_Comment by @konstin on 2025-06-22 13:59_

You can also install CPython yourself and disable managed Python downloads:

```
uv venv --no-python-downloads
```

---

_Closed by @charliermarsh on 2025-07-04 02:08_

---

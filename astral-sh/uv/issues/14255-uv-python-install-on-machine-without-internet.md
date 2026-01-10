---
number: 14255
title: uv python install on machine without internet access
type: issue
state: closed
author: rajatm91
labels:
  - question
assignees: []
created_at: 2025-06-25T08:58:33Z
updated_at: 2025-06-27T23:17:47Z
url: https://github.com/astral-sh/uv/issues/14255
synced_at: 2026-01-10T01:25:43Z
---

# uv python install on machine without internet access

---

_Issue opened by @rajatm91 on 2025-06-25 08:58_

### Question

Is it possible to use uv python install on a machine without internet access? If so, is there a way to provide a local path to a pre-downloaded Python distribution for installation?

### Platform

Ubuntu

### Version

uv 0.5.2

---

_Label `question` added by @rajatm91 on 2025-06-25 08:58_

---

_Comment by @Winand on 2025-06-25 15:01_

uv can detect any Python interpreter on the `PATH`, see [Discovery of Python versions](https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions).

Personally I build Python from source and install it manually on the machine without internet access into `~/.local/bin` (which is on the `PATH`), see [Winand/python-centos7-docker-build](https://github.com/Winand/python-centos7-docker-build).

---

_Comment by @nathanscain on 2025-06-25 15:58_

As you are asking for a way to reference predownloaded but not installed builds, I'd recommend using the `python-install-mirror` setting or `UV_PYTHON_INSTALL_MIRROR` envvar to tell uv where you have downloaded python build standalone builds.

You can also automate pulling builds of python using https://github.com/astral-sh/uv/blob/main/scripts/create-python-mirror.py and/or augment available versions shown with the `python-downloads-json-url` setting or `UV_PYTHON_DOWNLOADS_JSON_URL` envvar.

(The install mirror has existed for a while and should be supported perfectly in your version; however, the configuration of available versions is more new and would required an updated uv version.)

---

_Comment by @rajatm91 on 2025-06-26 06:34_

Thanks @nathanscain  for the answer, it worked.

---

_Closed by @rajatm91 on 2025-06-26 06:34_

---

_Comment by @nathanscain on 2025-06-27 20:43_

Happy to help!

---

_Comment by @zanieb on 2025-06-27 23:17_

There's also `UV_PYTHON_CACHE_DIR`

---

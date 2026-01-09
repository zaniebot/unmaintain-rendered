---
number: 11044
title: Can uv avoid downloading another uv when installing something that depends on uv?
type: issue
state: closed
author: cthoyt
labels:
  - question
assignees: []
created_at: 2025-01-28T23:11:44Z
updated_at: 2025-01-28T23:37:27Z
url: https://github.com/astral-sh/uv/issues/11044
synced_at: 2026-01-07T13:12:18-06:00
---

# Can uv avoid downloading another uv when installing something that depends on uv?

---

_Issue opened by @cthoyt on 2025-01-28 23:11_

### Question

I have `tox` installed as a tool with `tox-uv` to give it super speed. I noticed that any time uv is updated, and I run something like `uv tool update --all`, it re-downloads uv. 

Here's how to reproduce, though the download bars automatically delete themselves from the output, so I'm not sure how to retain that in the output:

```console
$ uv tool uninstall tox
error: `tox` is not installed
$ uv tool install tox --with tox-uv --no-cache
Resolved 13 packages in 261ms
Prepared 13 packages in 1.59s
Installed 13 packages in 26ms
 + cachetools==5.5.1
 + chardet==5.2.0
 + colorama==0.4.6
 + distlib==0.3.9
 + filelock==3.17.0
 + packaging==24.2
 + platformdirs==4.3.6
 + pluggy==1.5.0
 + pyproject-api==1.9.0
 + tox==4.24.1
 + tox-uv==1.20.2
 + uv==0.5.25
 + virtualenv==20.29.1
Installed 1 executable: tox
```

My question is: can uv have a fast path so it doesn't need to download uv again?

### Platform

Darwin 24.2.0 arm64

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

---

_Label `question` added by @cthoyt on 2025-01-28 23:11_

---

_Renamed from "Can uv avoid downloading another uv when installing somethign that depends on uv?" to "Can uv avoid downloading another uv when installing something that depends on uv?" by @cthoyt on 2025-01-28 23:11_

---

_Comment by @zanieb on 2025-01-28 23:29_

Hm. It seems sort of questionable to skip using the uv distribution from PyPI if a package says it requires it. The version could be different, for example. Or, it could rely on the uv Python module which is not present in binary distributions of uv.

---

_Comment by @cthoyt on 2025-01-28 23:35_

fair enough! feel free to mark this as closed

---

_Closed by @zanieb on 2025-01-28 23:37_

---

```yaml
number: 11634
title: Package-specific find-links, without using other indices
type: issue
state: closed
author: JWCS
labels:
  - enhancement
assignees: []
created_at: 2025-02-19T19:18:50Z
updated_at: 2025-03-26T01:14:45Z
url: https://github.com/astral-sh/uv/issues/11634
synced_at: 2026-01-10T03:41:47Z
```

# Package-specific find-links, without using other indices

---

_Issue opened by @JWCS on 2025-02-19 19:18_

### Question

Installing just one package from a find-links with pip was [(source)](https://mmcv.readthedocs.io/en/latest/get_started/installation.html#install-with-pip)
``` bash
pip install mmcv==2.2.0 -f https://download.openmmlab.com/mmcv/dist/cu121/torch2.4/index.html
```
I'm trying to replicate this functionality via the pyproject.toml configuration. 
The most that I can see allowed is:
(Validated with `uv lock --upgrade`)
``` toml
dependencies = [ "mmcv==2.2.0" ]
[tool.uv]
environments = [ "sys_platform == 'linux' and implementation_name == 'cpython' and platform_machine == 'x86_64'" ]
find-links = [ "https://download.openmmlab.com/mmcv/dist/cu121/torch2.4/index.html" ]
# ^ Is this is exposed to all other packages? Do all other package installs check this/these links?
# [tool.uv.sources]
# mmcv = { index = "mmcv-cu121" }
# [[tool.uv.index]]
# name = "mmcv-cu121"
# explicit = true
# url = "" # None? Exclude all other indices ? Not documented
```
Ideally, I'd only want the particular package to use that find-links, and I'd also want it to be excluded from searching other indices (ie pypi and other non-explicits). 
I might happen to have several other packages with this same install condition in the same project.
Am I just missing something in the documentation, or is this non-functionality?

### Platform

Ubuntu 20.04 amd64

### Version

uv 0.6.1

---

_Label `question` added by @JWCS on 2025-02-19 19:18_

---

_Comment by @zanieb on 2025-02-19 19:22_

This is on the roadmap to add as a mode to the `tool.uv.index` config but just hasn't been implemented yet, cc @charliermarsh 

---

_Label `question` removed by @zanieb on 2025-02-19 19:22_

---

_Label `enhancement` added by @zanieb on 2025-02-19 19:22_

---

_Comment by @zanieb on 2025-02-19 19:23_

To confirm, yes, using the top-level `tool.uv.find-links` will apply to other packages too.

---

_Comment by @charliermarsh on 2025-02-19 19:23_

Unfortunately the "lock a package to a specific index" functionality isn't available for `--find-links` yet. So what you have above is the best you can do. I can look into adding it. (We do support that functionality for "normal" registry indexes, but not for `--find-links`.)

(It sounds like you know this, but you can do `uv pip install mmcv==2.2.0 -f https://download.openmmlab.com/mmcv/dist/cu121/torch2.4/index.html` if you continue to use the lower-level `uv pip` API. But yeah, if you want to use the `pyproject.toml` and `uv.lock`-based APIs, it's not possible to do exactly what you've described right now.)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-24 01:59_

---

_Closed by @charliermarsh on 2025-03-26 01:14_

---

_Closed by @charliermarsh on 2025-03-26 01:14_

---

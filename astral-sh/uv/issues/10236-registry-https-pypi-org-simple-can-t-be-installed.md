```yaml
number: 10236
title: "@ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform"
type: issue
state: closed
author: Super1Windcloud
labels:
  - question
assignees: []
created_at: 2024-12-30T10:45:41Z
updated_at: 2024-12-30T15:31:01Z
url: https://github.com/astral-sh/uv/issues/10236
synced_at: 2026-01-12T16:00:09Z
```

# @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

---

_@Super1Windcloud_

##  When I  use  python 3.13  on  windows ,   and run  `uv add torch ` 
##  happened `@ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform` 



##  however  , When I use  python 3.12  , this error  is    disappeared and lost 

---

_Comment by @SauravMaheshkar on 2024-12-30 11:55_

I encountered the same error. The full actions log can be found [here](https://github.com/SauravMaheshkar/idefics-nnx/actions/runs/12546153305/job/34981501048)

---

_Comment by @charliermarsh on 2024-12-30 15:30_

This is correct. There are no Python 3.13 wheels for Torch on macOS or Windows.

---

_Comment by @charliermarsh on 2024-12-30 15:30_

See: https://pypi.org/project/torch/2.5.1/#files.

---

_Closed by @charliermarsh on 2024-12-30 15:31_

---

_Label `question` added by @charliermarsh on 2024-12-30 15:31_

---

---
number: 1533
title: "`uv pip install -r requirements.txt` does not actually check hashes"
type: issue
state: closed
author: hauntsaninja
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-02-16T19:34:10Z
updated_at: 2024-02-17T04:17:14Z
url: https://github.com/astral-sh/uv/issues/1533
synced_at: 2026-01-10T01:23:07Z
---

# `uv pip install -r requirements.txt` does not actually check hashes

---

_Issue opened by @hauntsaninja on 2024-02-16 19:34_

This is maybe a duplicate of https://github.com/astral-sh/uv/issues/474 , but I found this surprising. Are there any other big security TODOs like this?
```
λ cat r.txt
pypyp==1.1.0 \
    --hash=sha256:0000000000000000000000000000000000000000000000000000000000000000 \
    --hash=sha256:0000000000000000000000000000000000000000000000000000000000000000
λ pip install -r r.txt
Collecting pypyp==1.1.0 (from -r r.txt (line 3))
  Using cached pypyp-1.1.0-py3-none-any.whl (15 kB)
ERROR: THESE PACKAGES DO NOT MATCH THE HASHES FROM THE REQUIREMENTS FILE. If you have updated the package versions, please update the hashes. Otherwise, examine the package contents carefully; someone may have tampered with them.
    pypyp==1.1.0 from https://files.pythonhosted.org/packages/f1/66/1f65aeeffb447f89ef752f881bed76c47e9b0f2b4973433545ac7461ea95/pypyp-1.1.0-py3-none-any.whl (from -r r.txt (line 3)):
        Expected sha256 0000000000000000000000000000000000000000000000000000000000000000
        Expected     or 0000000000000000000000000000000000000000000000000000000000000000
             Got        55821b36f580c9ae5f3d179935327b053d3bb720e31140e8e97073b55c86c117

λ uv pip install -r r.txt
Resolved 1 package in 9ms
Installed 1 package in 24ms
 + pypyp==1.1.0
```

---

_Label `question` added by @zanieb on 2024-02-17 04:09_

---

_Comment by @zanieb on 2024-02-17 04:10_

I don't think there are other big security todos, other than supporting things like authentication robustly and perhaps innovating on extra index behavior.

Closing in favor of #474, we definitely want to do this.

---

_Closed by @zanieb on 2024-02-17 04:10_

---

_Label `duplicate` added by @zanieb on 2024-02-17 04:10_

---

_Comment by @charliermarsh on 2024-02-17 04:17_

The other one that comes to mind is that we don't currently validate hashes within the `RECORD` file of each wheel, although IIUC pip does not validate these either (https://github.com/pypa/pip/issues/4705).

---

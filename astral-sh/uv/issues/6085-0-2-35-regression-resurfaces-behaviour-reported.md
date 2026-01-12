```yaml
number: 6085
title: "0.2.35 - regression. Resurfaces behaviour reported in #3675"
type: issue
state: closed
author: paulJordaan
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-08-14T14:44:27Z
updated_at: 2024-08-20T20:06:18Z
url: https://github.com/astral-sh/uv/issues/6085
synced_at: 2026-01-12T15:59:01Z
```

# 0.2.35 - regression. Resurfaces behaviour reported in #3675

---

_@paulJordaan_

It seems like the behavior reported in [#3675](https://github.com/astral-sh/uv/issues/3675) has resurfaced after the 0.2.35 release.

I can see the code introduced as a [fix](https://github.com/astral-sh/uv/pull/3681) has been removed in a major refactor of the marker trees [here](https://github.com/astral-sh/uv/pull/5898).

```
root@8bc75a13208d:/# python -V
Python 3.11.9
root@8bc75a13208d:/# uv -V
uv 0.2.36
root@8bc75a13208d:/# pip -V
pip 24.0 from /usr/local/lib/python3.11/site-packages/pip (python 3.11)
root@8bc75a13208d:/# pip install --dry-run pickleshare
Collecting pickleshare
  Using cached pickleshare-0.7.5-py2.py3-none-any.whl.metadata (1.5 kB)
Using cached pickleshare-0.7.5-py2.py3-none-any.whl (6.9 kB)
Would install pickleshare-0.7.5

[notice] A new release of pip is available: 24.0 -> 24.2
[notice] To update, run: pip install --upgrade pip
root@8bc75a13208d:/# uv pip install --dry-run --system pickleshare
Resolved 3 packages in 18ms
Would download 3 packages
Would install 3 packages
 + pathlib2==2.3.7.post1
 + pickleshare==0.7.5
 + six==1.16.0
root@8bc75a13208d:/# 
```

---

_Comment by @zanieb on 2024-08-14 14:48_

Thanks for the report!

---

_Label `bug` added by @zanieb on 2024-08-14 14:48_

---

_Label `resolver` added by @zanieb on 2024-08-14 14:48_

---

_Comment by @charliermarsh on 2024-08-20 20:05_

I believe this is now fixed (in v0.3.0).

---

_Closed by @charliermarsh on 2024-08-20 20:06_

---

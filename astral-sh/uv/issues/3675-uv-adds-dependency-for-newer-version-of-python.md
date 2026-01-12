```yaml
number: 3675
title: uv adds dependency for newer version of python where pip does not
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
assignees: []
created_at: 2024-05-20T22:08:02Z
updated_at: 2024-05-21T01:01:12Z
url: https://github.com/astral-sh/uv/issues/3675
synced_at: 2026-01-12T15:58:45Z
```

# uv adds dependency for newer version of python where pip does not

---

_@notatallshaw-gts_

Upon reconciling the output of my uv and pip commands at work I found a curious additional dependency appear, I have narrowed it down, and I _think_ it's a bug in uv:

```
$ python -V
Python 3.11.2
$ uv -V
uv 0.1.45
$ pip -V
pip 24.0 from ...

$ pip install --dry-run pickleshare
Collecting pickleshare
  Using cached pickleshare-0.7.5-py2.py3-none-any.whl.metadata (1.5 kB)
Using cached pickleshare-0.7.5-py2.py3-none-any.whl (6.9 kB)
Would install pickleshare-0.7.5

$ uv pip install --dry-run pickleshare
Resolved 3 packages in 1ms
Would download 2 packages
Would install 2 packages
 + pathlib2==2.3.7.post1
 + pickleshare==0.7.5
```

uv adds this dependency `pathlib2` that pip does not.

Looking at the metadata (https://files.pythonhosted.org/packages/9a/41/220f49aaea88bc6fa6cba8d05ecf24676326156c23b991e80b3f2fc24c77/pickleshare-0.7.5-py2.py3-none-any.whl.metadata) this appears to be the offending line:

```
Requires-Dist: pathlib2; python_version in "2.6 2.7 3.2 3.3"
```

I've not had chance to read the spec for this, but appears that uv is wrong for requiring pathlib2 on Python 3.11.2.

---

_Label `bug` added by @zanieb on 2024-05-20 22:15_

---

_Comment by @zanieb on 2024-05-20 22:15_

Interesting, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-20 22:54_

---

_Closed by @charliermarsh on 2024-05-21 01:01_

---

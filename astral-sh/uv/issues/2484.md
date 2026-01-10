```yaml
number: 2484
title: Fail if req name and metadata name mismatch
type: issue
state: closed
author: wimglenn
labels:
  - bug
assignees: []
created_at: 2024-03-16T03:04:56Z
updated_at: 2024-03-18T17:35:16Z
url: https://github.com/astral-sh/uv/issues/2484
synced_at: 2026-01-10T05:40:32Z
```

# Fail if req name and metadata name mismatch

---

_Issue opened by @wimglenn on 2024-03-16 03:04_

pip refuses to install a .whl if the req is wrong, e.g.

```
$ pip install 'dateutil @ file:///tmp/python_dateutil-2.9.0.post0-py2.py3-none-any.whl'
Processing /tmp/python_dateutil-2.9.0.post0-py2.py3-none-any.whl
ERROR: dateutil has an invalid wheel, .dist-info directory 'python_dateutil-2.9.0.post0.dist-info' does not start with 'dateutil'
```

uv pip install should probably do the same? currently, it's happy with something like

```
uv pip install 'whatever @ file:///tmp/python_dateutil-2.9.0.post0-py2.py3-none-any.whl'
```



---

_Comment by @zanieb on 2024-03-16 03:27_

Thanks for the report. I imagine this would be annoying before we do #313, otherwise I'm all for it.

---

_Label `bug` added by @zanieb on 2024-03-16 03:27_

---

_Comment by @charliermarsh on 2024-03-16 13:16_

We enforce this in most cases — definitely when building source distributions. We probably need to add enforcement in a few codepaths. (I don’t think #313 is required.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-18 01:27_

---

_Closed by @charliermarsh on 2024-03-18 17:35_

---

---
number: 9657
title: "`uv export --script` being able to export the requirements of a script to a `requirements.txt`"
type: issue
state: closed
author: samster25
labels:
  - duplicate
assignees: []
created_at: 2024-12-05T07:36:22Z
updated_at: 2025-01-08T21:48:55Z
url: https://github.com/astral-sh/uv/issues/9657
synced_at: 2026-01-10T01:24:44Z
---

# `uv export --script` being able to export the requirements of a script to a `requirements.txt`

---

_Issue opened by @samster25 on 2024-12-05 07:36_

It would be very useful to be able to export the requirements from a script to a `requirements.txt` file. 

This is especially the case when you need to work with a tool that doesn't support `uv` and expects a `requirements.txt` to set up the environment like with ray.

```
# set up script
uv init --script example.py --python 3.12
# add numpy
uv add --script example.py 'numpy'

# this is what would be nice
uv export --script example.py > requirements.txt
```

---

_Comment by @my1e5 on 2024-12-05 10:21_

Duplicate of 
- https://github.com/astral-sh/uv/issues/8609

---

_Comment by @zanieb on 2024-12-05 15:11_

Thanks @my1e5 !

---

_Closed by @zanieb on 2024-12-05 15:11_

---

_Label `duplicate` added by @zanieb on 2024-12-05 15:11_

---

_Referenced in [astral-sh/uv#10160](../../astral-sh/uv/pulls/10160.md) on 2024-12-25 19:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-25 19:19_

---

_Closed by @charliermarsh on 2025-01-08 21:48_

---

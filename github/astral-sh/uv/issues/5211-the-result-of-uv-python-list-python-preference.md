---
number: 5211
title: "The result of `uv python list --python-preference only-system` shows unwanted python"
type: issue
state: closed
author: Di-Is
labels:
  - good first issue
  - cli
assignees: []
created_at: 2024-07-19T09:30:57Z
updated_at: 2024-07-19T14:15:33Z
url: https://github.com/astral-sh/uv/issues/5211
synced_at: 2026-01-07T13:12:17-06:00
---

# The result of `uv python list --python-preference only-system` shows unwanted python

---

_Issue opened by @Di-Is on 2024-07-19 09:30_

Running `uv python list --python-preference only-system` will show each minor version of Python with the description `<download available>`.

If `--python-preference only-system` is specified, I thought it would be better to show only Python installed on the system.

```bash
# Check system python
$ uv python --preview list --python-preference only-system
cpython-3.12.4-linux-x86_64-gnu     <download available>
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu     /bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /bin/python3
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     <download available>
cpython-3.7.9-linux-x86_64-gnu      <download available>
```

#### uv version

uv 0.2.26


---

_Renamed from "The result of `uv python list --python-preference only-system` shows unwanted Python." to "The result of `uv python list --python-preference only-system` shows unwanted python" by @Di-Is on 2024-07-19 09:32_

---

_Comment by @charliermarsh on 2024-07-19 12:42_

That seems reasonable to me. What do you think, @zanieb?

---

_Comment by @zanieb on 2024-07-19 13:47_

Yes we should do this, I thought we did but perhaps that was just respecting `--python-fetch`

---

_Label `good first issue` added by @zanieb on 2024-07-19 13:48_

---

_Label `cli` added by @zanieb on 2024-07-19 13:48_

---

_Referenced in [astral-sh/uv#5219](../../astral-sh/uv/pulls/5219.md) on 2024-07-19 14:07_

---

_Closed by @zanieb on 2024-07-19 14:15_

---

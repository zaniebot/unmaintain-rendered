---
number: 4091
title: "constraints not respected by `uv pip compile`"
type: issue
state: closed
author: iisakkirotko
labels:
  - bug
assignees: []
created_at: 2024-06-06T12:51:54Z
updated_at: 2024-06-06T14:00:02Z
url: https://github.com/astral-sh/uv/issues/4091
synced_at: 2026-01-10T01:23:34Z
---

# constraints not respected by `uv pip compile`

---

_Issue opened by @iisakkirotko on 2024-06-06 12:51_

If I run `uv pip compile req.txt -c constraints.txt`, with the two files having the following content:

req.txt
```
solara[extra]
```
where `pillow`'s version is not restricted at all in solara-ui's dependencies

constraints.txt
```
pillow==10.0.0
```

`uv` resolves
```
...
pillow==10.3.0
    # via
    #   -c constraints.txt
    #   solara-ui
...
```

despite the fact that the version should be constrained to 10.0.0.

This happens on `uv` versions 0.2.7 and 0.2.8, but versions <= 0.2.6 work as expected.

---

_Label `bug` added by @zanieb on 2024-06-06 12:56_

---

_Comment by @zanieb on 2024-06-06 12:57_

Thanks for reporting this regression! cc @charliermarsh / @konstin 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 13:06_

---

_Comment by @charliermarsh on 2024-06-06 13:34_

Thanks, not immediately obvious to me what changed but I'll fix and release it today.

---

_Referenced in [astral-sh/uv#4095](../../astral-sh/uv/pulls/4095.md) on 2024-06-06 13:50_

---

_Closed by @charliermarsh on 2024-06-06 13:58_

---

_Comment by @maartenbreddels on 2024-06-06 14:00_

FYI: we're using uv to solve environments for an in-browser python-sandbox for web apps, e.g. https://py.cafe/maartenbreddels/solara-dashboard-scatter (work in progress - yes, based on pyodide)

I thought it would be nice to hear how uv is being used. Thanks for this tool, and the quick fix!

---

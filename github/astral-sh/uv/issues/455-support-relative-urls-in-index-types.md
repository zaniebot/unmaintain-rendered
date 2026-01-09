---
number: 455
title: Support relative urls in index types
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-11-19T14:02:16Z
updated_at: 2023-12-27T13:53:22Z
url: https://github.com/astral-sh/uv/issues/455
synced_at: 2026-01-07T13:12:16-06:00
---

# Support relative urls in index types

---

_Issue opened by @konstin on 2023-11-19 14:02_

From [PEP 691](https://peps.python.org/pep-0691/#json-serialization):

> While JSON doesn’t natively support an URL type, any value that represents an URL in this API may be either absolute or relative as long as they point to the correct location. If relative, they are relative to the current URL as if it were HTML.

We currently assume absolute urls (as pypi always outputs absolute urls)

---

_Comment by @charliermarsh on 2023-12-01 19:35_

What is it relative to? The index URL?

---

_Comment by @konstin on 2023-12-01 19:46_

I can't say more than the PEP says, i think it's the same semantics as html has

---

_Comment by @charliermarsh on 2023-12-24 15:29_

This is supported for HTML indexes. We can add for JSON if it comes up.

---

_Comment by @zanieb on 2023-12-24 16:45_

Oh yeah, `devpi` does relative URLs if you want to test it there. I've been opting into absolute URLs when I start the server...

---

_Comment by @charliermarsh on 2023-12-24 17:05_

Thanks will do!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-24 17:05_

---

_Referenced in [astral-sh/uv#721](../../astral-sh/uv/pulls/721.md) on 2023-12-24 20:45_

---

_Closed by @charliermarsh on 2023-12-27 13:53_

---

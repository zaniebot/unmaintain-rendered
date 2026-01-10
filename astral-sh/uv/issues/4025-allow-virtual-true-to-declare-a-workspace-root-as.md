```yaml
number: 4025
title: "Allow `virtual = true` to declare a workspace root as virtual"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2024-06-04T21:54:16Z
updated_at: 2024-09-09T00:49:21Z
url: https://github.com/astral-sh/uv/issues/4025
synced_at: 2026-01-10T04:45:09Z
```

# Allow `virtual = true` to declare a workspace root as virtual

---

_Issue opened by @charliermarsh on 2024-06-04 21:54_

Right now, we seem to rely on `project` being absent, but that also means that we (e.g.) can't show a project name during errors. I think we should decouple `project` from virtual.

---

_Label `preview` added by @charliermarsh on 2024-06-04 21:54_

---

_Comment by @konstin on 2024-06-05 13:34_

Where would we get the package name from with `virtual = true`?

---

_Comment by @zanieb on 2024-06-05 13:55_

I presume there'd be a spec-compliant `[project]` table?

---

_Comment by @konstin on 2024-06-05 14:20_

The `[project]` is for specifying the (core) metadata of a distribution, but a virtual workspace is not a distribution. Through the `dynamic` mechanism, just specifying `name` and `version` for a `[project]` implies specifying the other metadata such as dependencies, too.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @charliermarsh on 2024-09-09 00:49_

We did this (`package = false`).

---

_Closed by @charliermarsh on 2024-09-09 00:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 00:49_

---

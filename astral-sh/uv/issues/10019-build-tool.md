```yaml
number: 10019
title: build tool
type: issue
state: closed
author: xhbbw
labels:
  - duplicate
assignees: []
created_at: 2024-12-19T02:56:24Z
updated_at: 2024-12-20T06:17:40Z
url: https://github.com/astral-sh/uv/issues/10019
synced_at: 2026-01-10T04:36:21Z
```

# build tool

---

_Issue opened by @xhbbw on 2024-12-19 02:56_

Can the project come up with a java-like maven packaging feature that can package a python project and its dependencies into a single file. The packaged file can be run directly using the python interpreter. I've used a few packaging tools: zipapp, shiv, pex. none of them are very good. I'm looking forward to seeing this feature in the project.

---

_Comment by @zanieb on 2024-12-19 04:01_

We're interested, but it's not on the immediate roadmap.

Duplicate of https://github.com/astral-sh/uv/issues/5802

---

_Closed by @zanieb on 2024-12-19 04:01_

---

_Label `duplicate` added by @zanieb on 2024-12-19 04:01_

---

_Comment by @xhbbw on 2024-12-20 06:17_

I mean just packaging the source code and dependencies of a python project into a package, like the standard library zipapp, but zipapp will have problems with dependencies not being found when packaging some three-way dependencies. For example, when using zipapp package to package a fastapi development project, there will be a problem that pydantic's three-way dependency can't be found

---

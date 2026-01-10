---
number: 5065
title: "Add no-op `uv lock` and `uv sync` overhead to codspeed benchmarks"
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-07-15T08:22:35Z
updated_at: 2024-08-23T13:39:23Z
url: https://github.com/astral-sh/uv/issues/5065
synced_at: 2026-01-10T01:23:45Z
---

# Add no-op `uv lock` and `uv sync` overhead to codspeed benchmarks

---

_Issue opened by @konstin on 2024-07-15 08:22_

We currently have some microbenchmarks and two resolver benchmarks. With the bluejay interface, most operations will involve no-op `uv lock` and `uv sync` operations when everything is already fresh. We should monitor their overhead with codspeed benchmarks.

---

_Label `performance` added by @konstin on 2024-07-15 08:22_

---

_Label `preview` added by @konstin on 2024-07-15 08:22_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @konstin on 2024-08-23 13:39_

These are very fast now, we don't need them in codspeed anymore i think.

---

_Closed by @konstin on 2024-08-23 13:39_

---

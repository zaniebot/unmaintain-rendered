```yaml
number: 8241
title: Non-deterministic snapshot in build test
type: issue
state: closed
author: zanieb
labels:
  - testing
assignees: []
created_at: 2024-10-16T04:14:24Z
updated_at: 2024-10-16T13:43:55Z
url: https://github.com/astral-sh/uv/issues/8241
synced_at: 2026-01-12T15:59:22Z
```

# Non-deterministic snapshot in build test

---

_@zanieb_

```
--- STDOUT:              uv::it build::tool_uv_sources ---

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: tool_uv_sources
Source: E:\uv:[1838](https://github.com/astral-sh/uv/actions/runs/11358695393/job/31593627442#step:9:1839)
───────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────
   18    18 │ writing dependency_links to project.egg-info/dependency_links.txt
   19    19 │ writing requirements to project.egg-info/requires.txt
   20    20 │ writing top-level names to project.egg-info/top_level.txt
   21    21 │ reading manifest file 'project.egg-info/SOURCES.txt'
   22       │-writing manifest file 'project.egg-info/SOURCES.txt'
   23    22 │ warning: sdist: standard file not found: should have one of README, README.rst, README.txt, README.md
   24    23 │ 
         24 │+writing manifest file 'project.egg-info/SOURCES.txt'
   25    25 │ running check
   26    26 │ creating project-0.1.0
   27    27 │ creating project-0.1.0/project.egg-info
   28    28 │ copying files to project-0.1.0...
```

---

_Label `testing` added by @zanieb on 2024-10-16 04:14_

---

_Assigned to @konstin by @zanieb on 2024-10-16 04:14_

---

_Assigned to @charliermarsh by @konstin on 2024-10-16 08:10_

---

_Unassigned @konstin by @konstin on 2024-10-16 08:10_

---

_Comment by @konstin on 2024-10-16 08:15_

Sounds like a race between stdout and stderr in setuptools output

---

_Comment by @charliermarsh on 2024-10-16 12:16_

It's odd that this hasn't come up in any other test! They all use the same form.

---

_Comment by @charliermarsh on 2024-10-16 12:17_

Oh, it's because that test is missing a README.

---

_Closed by @charliermarsh on 2024-10-16 12:26_

---

_Closed by @charliermarsh on 2024-10-16 12:26_

---

_Comment by @zanieb on 2024-10-16 13:43_

Thanks!

---

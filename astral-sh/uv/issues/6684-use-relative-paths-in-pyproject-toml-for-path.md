```yaml
number: 6684
title: Use relative paths in pyproject.toml for path dependencies
type: issue
state: closed
author: harrymconner
labels:
  - bug
  - cli
assignees: []
created_at: 2024-08-27T12:52:16Z
updated_at: 2024-08-27T14:02:10Z
url: https://github.com/astral-sh/uv/issues/6684
synced_at: 2026-01-10T04:45:09Z
```

# Use relative paths in pyproject.toml for path dependencies

---

_Issue opened by @harrymconner on 2024-08-27 12:52_

My company organizes its python code in a monorepo, and the recent addition of relative paths in lockfiles (#6490) convinced me to switch to uv! I was wondering if it would also be possible to use relative paths in the pyproject.toml file when `uv add`-ing path dependencies? Here is the repo structure for reference:
```
root
├-libs
│  ├-liba
│  └-libb
└-projects
   └-projecta
```
I want to add `libb` as a dependency to projecta, so I run the following command: `uv add ..\..\libs\libb --editable` from projecta. This gives me the following `[tool.uv.sources]` section in projecta's pyproject.toml:
```toml
[tool.uv.sources]
libb = { path = 'C:\full\path\to\the\library\libb', editable = true }
```
Since all of my team members have the monorepo checked out to different locations on their machine, I would like to have the dependency added as a relative path for consistency:
```toml
[tool.uv.sources]
libb = { path = '..\..\libs\libb', editable = true }
```

Please let me know if you have any questions on this use case!

Relevant info:
* Platform: Windows 11
* version: uv 0.3.4 (39f3cd2a9 2024-08-26)

---

_Comment by @charliermarsh on 2024-08-27 12:53_

I think this makes sense.

---

_Label `bug` added by @charliermarsh on 2024-08-27 12:53_

---

_Label `cli` added by @charliermarsh on 2024-08-27 12:53_

---

_Comment by @charliermarsh on 2024-08-27 12:54_

(I'm gonna call it a bug since it probably should've always behaved this way.)

---

_Closed by @charliermarsh on 2024-08-27 14:02_

---

_Closed by @charliermarsh on 2024-08-27 14:02_

---

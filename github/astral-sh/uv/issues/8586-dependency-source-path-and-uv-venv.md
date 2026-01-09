---
number: 8586
title: Dependency source path and uv venv
type: issue
state: closed
author: CharlesPerrotMinot
labels: []
assignees: []
created_at: 2024-10-26T04:54:14Z
updated_at: 2024-10-28T04:59:56Z
url: https://github.com/astral-sh/uv/issues/8586
synced_at: 2026-01-07T13:12:18-06:00
---

# Dependency source path and uv venv

---

_Issue opened by @CharlesPerrotMinot on 2024-10-26 04:54_

Hi,

I think there's an issue when switching from a local source (`path=`) to an index one (`index=`).
Say I have a dependency in an index `dependency = { index = "myindex" }`

In the virtual environment, I get:
```
$ uv pip show dependency
Name: dependency
Version: 2.5.40
(...)

$ uv pip list -e --format json
[]
```

If I put as an editable local, it works all good
`dependency = { index = "../../libraries/dependency" }`

In the virtual environment, I get:
```
$ uv pip show dependency
Name: dependency
Version: 2.5.40
Editable project location: /Users/cperrotminot/Workspace/libraries/dependency
(...)

$ uv pip list -e --format json
[{"name":"dependency","version":"2.5.40","editable_project_location":"/Users/cperrotminot/Workspace/libraries/dependency"}]
```

But if I change it back to an index, the info stays the same (aka shown as local)
```
$ uv pip show dependency
Name: dependency
Version: 2.5.40
Editable project location: /Users/cperrotminot/Workspace/libraries/dependency
(...)

$ uv pip list -e --format json
[{"name":"dependency","version":"2.5.40","editable_project_location":"/Users/cperrotminot/Workspace/libraries/dependency"}]
```


The only way to fix the issue that I found is to rebuild the virtual environment (`uv venv`, without `--allow-existing`).
Thanks!

---

_Assigned to @charliermarsh by @zanieb on 2024-10-26 16:20_

---

_Comment by @charliermarsh on 2024-10-26 16:27_

Are you running an install command between those? uv pip list just tells you what’s currently installed.

---

_Comment by @CharlesPerrotMinot on 2024-10-28 04:59_

Hum.
- I'm running `uv lock` and `uv sync`
- For some reason I cannot reproduce at all. I was sourcing the environments, got the issue even doing `uv run`, but now it works all the time, with or without `uv venv`.

So... sorry :( And thanks for looking into this!

---

_Closed by @CharlesPerrotMinot on 2024-10-28 04:59_

---

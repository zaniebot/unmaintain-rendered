```yaml
number: 17140
title: sub-projects in uv?
type: issue
state: closed
author: M-Lauronen
labels:
  - question
assignees: []
created_at: 2025-12-16T07:48:53Z
updated_at: 2025-12-16T11:47:44Z
url: https://github.com/astral-sh/uv/issues/17140
synced_at: 2026-01-12T16:02:44Z
```

# sub-projects in uv?

---

_@M-Lauronen_

### Question

What is uv's recommended approach for implementing sub-projects, for example, in a situation where I want to use the same virtual environment and project-level global modules across multiple applications within the project?

> Project/
> ├─.venv
> ├─modules
> ├── app1/
> │   └── src/
> ├── app2/
> │   └── src/
> └── app3/
>     └── src/
> 

### Platform

Linux Debian

### Version

Latest possible uv version

---

_Label `question` added by @M-Lauronen on 2025-12-16 07:48_

---

_Comment by @konstin on 2025-12-16 09:45_

uv supports workspaces for this: https://docs.astral.sh/uv/concepts/projects/workspaces/

---

_Comment by @M-Lauronen on 2025-12-16 11:47_

Thank you very much. That was very valuable information. It is exactly what I was looking for.

I switched to 'uv'.

---

_Closed by @M-Lauronen on 2025-12-16 11:47_

---

```yaml
number: 7778
title: In case VIRTUAL_ENV is different from .venv UV_PROJECT_ENVIRONMENT should be created automatically
type: issue
state: closed
author: avoutsas67
labels:
  - question
assignees: []
created_at: 2024-09-29T10:40:50Z
updated_at: 2025-01-06T15:42:30Z
url: https://github.com/astral-sh/uv/issues/7778
synced_at: 2026-01-12T15:59:16Z
```

# In case VIRTUAL_ENV is different from .venv UV_PROJECT_ENVIRONMENT should be created automatically

---

_@avoutsas67_

Hi all, 

If I understand correctly, the current behaviour of **uv add** when `VIRTUAL_ENVIRONMENT` is different than `.venv `is to raise a warning and create a .venv environment where the dependency will be created. 
`warning: `VIRTUAL_ENV=.venv-py312` does not match the project environment path `.venv` and will be ignored
Using Python 3.12.6 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv`

I believe it would be more intuitive and more efficient when the `VIRTUAL_ENV` is different from .venv that the `UV_PROJECT_ENVIRONMENT` is set automatically to `VIRTUAL_ENV` by **uv init**. This will remove an extra step in setting up a project.

If a users wants to use a different environment from the one set in `VIRTUAL_ENV` then they could explicitly set the `UV_PROJECT_ENVIRONMENT`.

Thanks for considering this request.

Achilleas


---

_Comment by @zanieb on 2024-09-29 14:47_

We intentionally don't support using the "active" virtual environment for a project.

More context in #6834 which links to much more discussion. 

---

_Comment by @sglbl on 2024-11-29 09:22_

@zanieb So how can I use my cpu-based-venv without removing the other one?

![Image](https://github.com/user-attachments/assets/ea631eca-2d86-42f7-8df9-64cdd598f6ac)


---

_Comment by @adhadse on 2024-12-28 05:16_

Unfortunately, this doesn't allow me to keep all virtual environments at one place under `~` dir. 

Currently `UV_PROJECT_ENVIRONMENT` only allow me to configure for one project.

I usually name project environments with the same name as project and keep them under one folder in `~/uv_environments`.

---

_Comment by @zanieb on 2024-12-28 16:53_

@adhadse that's tracked in #1495 

@sglbl we don't really support that yet. You can hack it with `UV_PROJECT_ENVIRONMENT` but there's not a way to define multiple persistent environments. We're thinking about it, but there can be 2^N environments (N = number of groups + extras + resolution modes) for each project and we don't want to just store every combination ever created.

---

_Label `question` added by @zanieb on 2024-12-28 16:53_

---

_Comment by @mgu on 2025-01-04 10:12_

@adhadse for this specific use case, I use `direnv` and export UV_PROJECT_ENVIRONMENT in my `.envrc`. I don't know if this is something that could help you

---

_Comment by @adhadse on 2025-01-06 06:41_

Right now, I'm not interested in fiddling with this issue. I'm going with how uv does it. I'll come back to it later once I start using uv  with more and more projects.

---

_Closed by @zanieb on 2025-01-06 15:31_

---

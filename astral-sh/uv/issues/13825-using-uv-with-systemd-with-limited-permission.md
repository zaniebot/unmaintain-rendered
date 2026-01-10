---
number: 13825
title: Using uv with systemd with limited permission users.
type: issue
state: open
author: mehdipourfar
labels:
  - question
assignees: []
created_at: 2025-06-03T18:01:54Z
updated_at: 2025-06-04T17:30:17Z
url: https://github.com/astral-sh/uv/issues/13825
synced_at: 2026-01-10T01:25:38Z
---

# Using uv with systemd with limited permission users.

---

_Issue opened by @mehdipourfar on 2025-06-03 18:01_

### Question

For many years, I've used virtualenv. I used to create a venv directory in the /opt/virtualenvs path and deploy services using systemd with the www-data (nginx default) user and group.

I've migrated one of my projects to uv. The problem is that uv scatters packages across different directories: one directory for Python installations, one directory for the uv command, one directory for uv cache, and also .venv in the project directory. I have to grant permissions to different directories for the www-data user.

I want to know: if I don't want to use Docker, what is the correct way of deploying a service using systemd?

### Platform

Linux 5.15.0-113-generic x86_64 GNU/Linux

### Version

uv 0.7.10

---

_Label `question` added by @mehdipourfar on 2025-06-03 18:01_

---

_Comment by @konstin on 2025-06-04 08:30_

uv follow the XDG spec, so one option is to give the www-data user the usual XDG directories (cache, config and data). You can also configure the locations that uv uses manually, with `UV_PYTHON_INSTALL_DIR` and `UV_CACHE_DIR`.

---

_Comment by @woutervh on 2025-06-04 17:30_

I'm not the biggest fan of docker either.

I like to supersize my virtualenvs to isolated application directories where everything gets installed inside the project-dir
using the env-variables:

```
 /opt/<project-dir>
     .venv/
     var/cache/uv
     var/python
     var/tmp
```

---

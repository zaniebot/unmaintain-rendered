```yaml
number: 6809
title: Windows _netrc not recognized for pip auth
type: issue
state: closed
author: corby
labels:
  - bug
  - windows
assignees: []
created_at: 2024-08-29T12:39:06Z
updated_at: 2024-10-08T22:15:40Z
url: https://github.com/astral-sh/uv/issues/6809
synced_at: 2026-01-12T15:59:07Z
```

# Windows _netrc not recognized for pip auth

---

_@corby_

I cross compile a python application with lots of C libraries.
uv works fine on *nix based platforms, but on Windows the ~/.netrc file is called ~/_netrc (who knows why)

Copying the ~/_netrc to ~/.netrc fixed uv.
Can we have uv scan for ~/.netrc and fall back to ~/_netrc on windows platform?

---

_Comment by @zanieb on 2024-08-29 13:40_

Hm I presumed support was added when they did upstream https://github.com/gribouille/netrc/issues/1 but maybe not. We should follow-up here.

---

_Label `bug` added by @zanieb on 2024-08-29 13:40_

---

_Label `windows` added by @zanieb on 2024-08-29 13:40_

---

_Closed by @zanieb on 2024-10-08 22:15_

---

_Closed by @zanieb on 2024-10-08 22:15_

---

```yaml
number: 5623
title: "`uvx` should note if no executables are provided by a package"
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-07-30T18:09:59Z
updated_at: 2024-07-31T22:27:42Z
url: https://github.com/astral-sh/uv/issues/5623
synced_at: 2026-01-12T15:58:57Z
```

# `uvx` should note if no executables are provided by a package

---

_@zanieb_

e.g.

```
❯ uvx requests
warning: `uvx` is experimental and may change without warning
Resolved 5 packages in 102ms
Installed 5 packages in 10ms
 + certifi==2024.7.4
 + charset-normalizer==3.3.2
 + idna==3.7
 + requests==2.32.3
 + urllib3==2.2.2
warning: An executable named `requests` is not provided by package `requests`.
The executable `requests` was not found.
```

I'm left guessing if `requests` happens to provide another executable.

---

_Label `error messages` added by @zanieb on 2024-07-30 18:10_

---

_Closed by @zanieb on 2024-07-31 22:27_

---

_Closed by @zanieb on 2024-07-31 22:27_

---

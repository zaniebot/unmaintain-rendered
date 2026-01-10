---
number: 15587
title: publishing to azure artifacts is broken
type: issue
state: closed
author: niekrongen
labels:
  - bug
assignees: []
created_at: 2025-08-29T22:51:22Z
updated_at: 2025-08-29T23:49:47Z
url: https://github.com/astral-sh/uv/issues/15587
synced_at: 2026-01-10T01:25:57Z
---

# publishing to azure artifacts is broken

---

_Issue opened by @niekrongen on 2025-08-29 22:51_

### Summary

in uv0.8.14 the publish functionality for python libraries to azure artifacts is not working.
uv 0.8.13 does not have any issues. 

The following error occurs when trying to publish

```
Publishing 2 files https://pkgs.dev.azure.com/<org>/<project>/_packaging/<feed>/pypi/upload/
error: Failed to query check URL
  Caused by: Failed to fetch: `[https://pkgs.dev.azure.com/<org>/<project>/_packaging/<feed>/pypi/simple/sifters/`](https://pkgs.dev.azure.com/<org>/<project>/_packaging/<feed>/pypi/simple/<org>/%60)
  Caused by: Missing credentials for https://pkgs.dev.azure.com/<org>/<project>/_packaging/<feed>/pypi/simple/<org>/

##[error]Bash exited with code '2'.
```

### Platform

ubuntu-latest 24.04.3

### Version

uv 0.8.14

### Python version

Python 3.11.13

---

_Label `bug` added by @niekrongen on 2025-08-29 22:51_

---

_Comment by @charliermarsh on 2025-08-29 23:49_

I think this was solved by #15546.

---

_Closed by @charliermarsh on 2025-08-29 23:49_

---

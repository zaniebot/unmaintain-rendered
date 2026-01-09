---
number: 20941
title: FAST003 syntax error when a path parameter is a Python keyword
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-17T15:34:56Z
updated_at: 2025-10-20T23:37:22Z
url: https://github.com/astral-sh/ruff/issues/20941
synced_at: 2026-01-07T13:12:16-06:00
---

# FAST003 syntax error when a path parameter is a Python keyword

---

_Issue opened by @dscorbett on 2025-10-17 15:34_

### Summary

The fix for [`fast-api-unused-path-parameter` (FAST003)](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter/) introduces a syntax error when the path parameter is a Python keyword or `__debug__`. The diagnostic is still correct but it can’t be fixed automatically. [Example](https://play.ruff.rs/7ddc1f47-60ed-44a7-8154-1152c1e45c6a):
```console
$ cat >fast003.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/imports/{import}")
async def get_import(): ...
# EOF

$ ruff --isolated check fast003.py --select FAST003 --unsafe-fixes --output-format concise -q
fast003.py:3:20: FAST003 [*] Parameter `import` appears in route path, but not in `get_import` signature

$ ruff --isolated check fast003.py --select FAST003 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.14.1 (2bffef596 2025-10-16)

---

_Label `bug` added by @ntBre on 2025-10-17 17:40_

---

_Label `fixes` added by @ntBre on 2025-10-17 17:40_

---

_Comment by @ntBre on 2025-10-17 17:42_

Thanks! Yeah we should just skip the fix in that case. We could play with adding a sub-diagnostic explaining why (e.g. `info: Fix is unavailable because the parameter name conflicts...`) but that's probably unnecessary.

---

_Comment by @TaKO8Ki on 2025-10-17 19:06_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-10-17 19:09_

---

_Referenced in [astral-sh/ruff#20960](../../astral-sh/ruff/pulls/20960.md) on 2025-10-18 15:58_

---

_Closed by @amyreese on 2025-10-20 23:37_

---

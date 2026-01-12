```yaml
number: 19831
title: FAST003 fix introduces syntax errors
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-08T16:29:37Z
updated_at: 2025-08-08T22:36:34Z
url: https://github.com/astral-sh/ruff/issues/19831
synced_at: 2026-01-12T15:54:57Z
```

# FAST003 fix introduces syntax errors

---

_@dscorbett_

### Summary

The fix for [`fast-api-unused-path-parameter` (FAST003)](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter/) can introduce a syntax error.

When the function’s first parameter uses `*`, FAST003 doesn’t add a comma before it. [Example](https://play.ruff.rs/41ea27f0-b193-4d35-b28b-b9f57e993960):
```console
$ cat >fast003_1.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/things/{thing_id}")
async def read_thing(*query: str): ...
# EOF

$ ruff --isolated check fast003_1.py --select FAST003 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

Ditto for `**`. [Example](https://play.ruff.rs/fea88b1d-638a-41f4-bed2-c272bb5cdbc3):
```console
$ cat >fast003_2.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/things/{thing_id}")
async def read_thing(**query: str): ...
# EOF

$ ruff --isolated check fast003_2.py --select FAST003 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

When the parameter list ends with `/`, FAST003 puts the new parameter before it, introducing a syntax error. [Example](https://play.ruff.rs/77413da4-f03c-4b6d-9da2-b19654642847):
```console
$ cat >fast003_3.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/queries/{thing_id}")
async def read_query(query: str, /): ...
# EOF

$ ruff --isolated check fast003_3.py --select FAST003 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The fix introduces a syntax error when the path parameter name is `__debug__`. It detects the error but modifies the file anyway. The fix should be suppressed in this case, as it already is for path parameters that are Python keywords. [Example](https://play.ruff.rs/dd4f64ea-3a37-4483-806e-4feadf369eca):
```console
$ cat >fast003_4.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/queries/{__debug__}")
async def read_query(query: str): ...
# EOF

$ ruff --isolated check fast003_4.py --select FAST003 --unsafe-fixes --fix --output-format concise -q
fast003_4.py:4:34: invalid-syntax: cannot assign to `__debug__`

$ cat fast003_4.py
from fastapi import FastAPI
app = FastAPI()
@app.get("/queries/{__debug__}")
async def read_query(query: str, __debug__): ...

$ python fast003_4.py 2>&1 | tail -n 1
SyntaxError: cannot assign to __debug__
```

The fix fails to converge when the path parameter is equivalent under NFKC to a function parameter but not equal to it. [FastAPI only supports ASCII in path parameter names anyway](https://github.com/encode/starlette/blob/0.47.2/starlette/routing.py#L121), so the fix should ignore non-ASCII path parameters. [Example](https://play.ruff.rs/0ef76e8d-be98-430f-8a84-ae28ec1f3e1c):
```console
$ cat >fast003_5.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/queries/{𝑞𝑢𝑒𝑟𝑦}")
async def read_query(query: str): ...
# EOF

$ ruff --isolated check fast003_5.py --select FAST003 --unsafe-fixes --diff

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `fast003_5.py`, the rule codes FAST003, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

--- fast003_5.py
+++ fast003_5.py
@@ -1,4 +1,4 @@
 from fastapi import FastAPI
 app = FastAPI()
 @app.get("/queries/{𝑞𝑢𝑒𝑟𝑦}")
-async def read_query(query: str): ...
+async def read_query(query: str, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦, 𝑞𝑢𝑒𝑟𝑦): ...

Would fix 100 errors.
```

The fix can add a parameter without a default after a positional-only parameter with a default, which is a syntax error. [Example](https://play.ruff.rs/1854c0ba-e16b-4c58-badb-fc6f9c874bfd):
```console
$ cat >fast003_6.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/things/{thing_id}")
async def read_thing(query: str = "", /,): ...
# EOF

$ ruff --isolated check fast003_6.py --select FAST003 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The fix can create a positional-only parameter. That isn’t a syntax error but it doesn’t fix the problem, so it is not a useful change. As the FAST003 documentation says, “If a path parameter is declared in the route path, but as a positional-only argument in the function signature, it will also not be accessible in the function body, as FastAPI will not inject the parameter.” [Example](https://play.ruff.rs/9e05f449-568b-46d1-9ec3-545657264cf9):
```console
$ cat >fast003_7.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/things/{thing_id}")
async def read_thing(query: str = "", /, x=None): ...
# EOF

$ ruff --isolated check fast003_7.py --select FAST003 --unsafe-fixes --fix --output-format concise -q
fast003_7.py:3:19: FAST003 Parameter `thing_id` appears in route path, but only as a positional-only argument in `read_thing` signature

$ cat fast003_7.py 
from fastapi import FastAPI
app = FastAPI()
@app.get("/things/{thing_id}")
async def read_thing(thing_id, query: str = "", /, x=None): ...
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `bug` added by @ntBre on 2025-08-08 18:46_

---

_Label `fixes` added by @ntBre on 2025-08-08 18:46_

---

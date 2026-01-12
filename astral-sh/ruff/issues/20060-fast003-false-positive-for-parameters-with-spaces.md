```yaml
number: 20060
title: "FAST003 false positive for parameters with spaces like `/{ x }`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-25T01:02:40Z
updated_at: 2025-08-29T13:40:27Z
url: https://github.com/astral-sh/ruff/issues/20060
synced_at: 2026-01-12T15:54:57Z
```

# FAST003 false positive for parameters with spaces like `/{ x }`

---

_@dscorbett_

### Summary

[`fast-api-unused-path-parameter` (FAST003)](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter/) trims spaces from parameters in a path, treating e.g. `/{ x }` as `/{x}`, but this leads to false positives because [FastAPI does not recognize spaces around parameters](https://github.com/encode/starlette/blob/0.47.3/starlette/routing.py#L121). FAST003 should only apply when FastAPI would detect a parameter. [The following example](https://play.ruff.rs/f520e4b9-c4ee-4627-9e9d-56bfd6e5aed2) shows that FastAPI treats `/{ x }` literally and that the function parameter introduced by FAST003’s fix is detected as a query parameter, not as part of the path.

```console
$ cat >fast003.py <<'# EOF'
from fastapi import FastAPI
app = FastAPI()
@app.get("/{ x }")
async def f():
    return {"x": x}
# EOF

$ pip install -q 'fastapi[standard]'

$ fastapi dev fast003.py >/dev/null 2>&1

$ curl http://127.0.0.1:8000/foo
{"detail":"Not Found"}

$ curl 'http://127.0.0.1:8000/%7B%20x%20%7D'
Internal Server Error

$ ruff --isolated check fast003.py --select FAST003 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ fastapi dev fast003.py >/dev/null 2>&1

$ curl http://127.0.0.1:8000/foo
{"detail":"Not Found"}

$ curl 'http://127.0.0.1:8000/%7B%20x%20%7D'
{"detail":[{"type":"missing","loc":["query","x"],"msg":"Field required","input":null}]}

$ curl 'http://127.0.0.1:8000/%7B%20x%20%7D?x=1'
{"x":"1"}
```

### Version

ruff 0.12.10 (c68ff8d90 2025-08-21)

---

_Label `bug` added by @ntBre on 2025-08-25 12:46_

---

_Label `rule` added by @ntBre on 2025-08-25 12:46_

---

_Closed by @dylwil3 on 2025-08-29 13:40_

---

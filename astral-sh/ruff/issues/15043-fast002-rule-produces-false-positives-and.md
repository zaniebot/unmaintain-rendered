```yaml
number: 15043
title: "`FAST002` rule produces false positives and incorrect suggestions"
type: issue
state: closed
author: MarkusSintonen
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-12-18T08:04:38Z
updated_at: 2025-01-15T18:05:55Z
url: https://github.com/astral-sh/ruff/issues/15043
synced_at: 2026-01-10T11:09:56Z
```

# `FAST002` rule produces false positives and incorrect suggestions

---

_Issue opened by @MarkusSintonen on 2024-12-18 08:04_

Ruff (0.8.3) `FAST002` produces false positives and suggests incorrect `Annotated` format with FastAPI params having default values. Following example can not be converted into the `Annotated` format as FastAPI does not support this format with params having default values. Also `--unsafe-fixes` fixes it to the format the FastAPI wont accept.

Ruff FAST002 violation:
```python
def test_fast002_bug():
    from fastapi import FastAPI, Query
    app = FastAPI()

    @app.get("/test")
    def handler(echo: str = Query("")):  # FAST002 FastAPI dependency without `Annotated`
        return echo
```

But that is not compatible with FastAPI. It fails deeply with an AssertionError:
```python
def test_fast002_bug():
    from typing import Annotated
    from fastapi import FastAPI, Query
    app = FastAPI()

    # AssertionError: `Query` default value cannot be set in `Annotated` for 'echo'. Set the default value with `=` instead.
    @app.get("/test")
    def handler(echo: Annotated[str, Query("")]):
        return echo
```

Used FastAPI 0.115.5.

The rule is pretty nice otherwise so thanks for it! Hopefully we get it fixed.

---

_Comment by @MichaReiser on 2024-12-18 08:43_

Thanks for opening the issue. I think this is a true positive, but the fix is incorrect. The `Query` default value cannot be used inside `Annotated` according to [FastAPI's documentation](https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#query-as-the-default-value-or-in-annotated). Instead, a regular default value should be used. So the proper fix for your example is to rewrite it to 


```py
def test_fast002_bug():
    from typing import Annotated
    from fastapi import FastAPI, Query
    app = FastAPI()

    # AssertionError: `Query` default value cannot be set in `Annotated` for 'echo'. Set the default value with `=` instead.
    @app.get("/test")
    def handler(echo: Annotated[str, Query()] = ""):
        return echo
```

---

_Label `bug` added by @MichaReiser on 2024-12-18 08:43_

---

_Label `fixes` added by @MichaReiser on 2024-12-18 08:43_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-10 13:20_

---

_Assigned to @ntBre by @MichaReiser on 2025-01-14 07:50_

---

_Unassigned @AlexWaygood by @MichaReiser on 2025-01-14 07:50_

---

_Closed by @ntBre on 2025-01-15 18:05_

---

_Closed by @ntBre on 2025-01-15 18:05_

---

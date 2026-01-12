```yaml
number: 9777
title: Running Different Application Frameworks with uv Command
type: issue
state: closed
author: HeangSok-2
labels:
  - question
assignees: []
created_at: 2024-12-10T15:40:50Z
updated_at: 2025-01-07T19:21:27Z
url: https://github.com/astral-sh/uv/issues/9777
synced_at: 2026-01-12T15:59:58Z
```

# Running Different Application Frameworks with uv Command

---

_@HeangSok-2_

At the moment, when we run our application using the uv command, for example:
````bash
uv run --package app1 apps/app1/main.py
````
The question arises: What if app1 utilizes Google Cloud Functions Framework or FastAPI? Can we adapt the command to handle these frameworks? For instance:

1. For Google Cloud Functions Framework:
````bash
uv run --package app1 "functions-framework --source apps/app1/main.py --target main --port 8080"
```` 
2. For FastAPI:
````bash
uv run --package app1 "uvicorn apps.app1.main:app --workers 1 --host 0.0.0.0 --port 8000 --reload"
````

This approach could streamline development while accommodating various frameworks in a separate sandbox. Is this feasible?

---

_Comment by @zanieb on 2025-01-07 19:21_

Yeah, use `dependency-groups` or `extras` or `--with <pkg>` for an ad-hoc dependency

https://docs.astral.sh/uv/concepts/projects/dependencies/

---

_Closed by @zanieb on 2025-01-07 19:21_

---

_Label `question` added by @zanieb on 2025-01-07 19:21_

---

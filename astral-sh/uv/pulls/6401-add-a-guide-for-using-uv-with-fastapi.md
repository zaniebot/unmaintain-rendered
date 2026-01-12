```yaml
number: 6401
title: Add a guide for using uv with FastAPI
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/fastapi
created_at: 2024-08-22T02:44:16Z
updated_at: 2024-08-23T02:09:46Z
url: https://github.com/astral-sh/uv/pull/6401
synced_at: 2026-01-12T16:07:21Z
```

# Add a guide for using uv with FastAPI

---

_@charliermarsh_

_No description provided._

---

_Review requested from @zanieb by @charliermarsh on 2024-08-22 02:44_

---

_Label `documentation` added by @charliermarsh on 2024-08-22 02:44_

---

_Marked ready for review by @charliermarsh on 2024-08-22 02:44_

---

_Review comment by @tiangolo on `docs/guides/integration/fastapi.md`:59 on 2024-08-22 03:12_

```suggestion
    "fastapi[standard]",
```

---

_Review comment by @tiangolo on `docs/guides/integration/fastapi.md`:70 on 2024-08-22 03:14_

```suggestion
$ uv run fastapi dev app/main.py
```

This could use the FastAPI CLI. Later I'll support configuring the app in the same `pyproject.toml` so those settings will be able to live in the same file.

---

_Review comment by @tiangolo on `docs/guides/integration/fastapi.md`:85 on 2024-08-22 03:15_

```suggestion
$ uv add "fastapi[standard]"
```

---

_Review comment by @tiangolo on `docs/guides/integration/fastapi.md`:112 on 2024-08-22 03:15_

```suggestion
$ uv run fastapi dev src/app/main.py
```

---

_@tiangolo reviewed on 2024-08-22 03:17_

Love this! :heart_eyes: 

I have a couple of suggestions.

One, installing `fastapi[standard]`, that includes `fastapi-cli`, `uvicorn`, `uvloop`, `httptools`, etc.

Two, using the `fastapi` CLI.

---

_@zanieb reviewed on 2024-08-22 04:11_

---

_Review comment by @zanieb on `docs/guides/integration/fastapi.md`:1 on 2024-08-22 04:11_

I was thinking we'd do "Building a REST API" and "Building a CLI" top-level guides — maybe after "Working on projects" and probably both just using whatever we feel like is good practice e.g. FastAPI and Typer(?). I guess they could go in the integration guides instead, but it seems nice to frame it around the general idea rather than the specific framework (even if we use a specific framework in the example). What do you think?

---

_@zanieb reviewed on 2024-08-22 04:13_

---

_Review comment by @zanieb on `docs/guides/integration/fastapi.md`:1 on 2024-08-22 04:13_

(will review the rest tomorrow)

---

_Review comment by @rbange on `docs/guides/integration/fastapi.md`:120 on 2024-08-22 08:11_

```suggestion
FROM python:3.12-slim
```

I am afraid this will be copied a lot so maybe just unpin the Debian version. Otherwise bookworm is the latest version.

---

_@rbange reviewed on 2024-08-22 08:11_

---

_@charliermarsh reviewed on 2024-08-22 12:41_

---

_Review comment by @charliermarsh on `docs/guides/integration/fastapi.md`:1 on 2024-08-22 12:41_

I prefer having something targeted like this. We could always expand it later if needed.

---

_Review comment by @samypr100 on `docs/guides/integration/fastapi.md`:33 on 2024-08-22 13:12_

```suggestion
```plaintext
```

---

_@samypr100 reviewed on 2024-08-22 13:12_

---

_@elio2t reviewed on 2024-08-22 13:43_

---

_Review comment by @elio2t on `docs/guides/integration/fastapi.md`:85 on 2024-08-22 13:43_

`uv add fastapi --extra standard`

---

_@zanieb approved on 2024-08-22 23:21_

Looks reasonable to me.

---

_Merged by @charliermarsh on 2024-08-23 02:09_

---

_Closed by @charliermarsh on 2024-08-23 02:09_

---

_Branch deleted on 2024-08-23 02:09_

---

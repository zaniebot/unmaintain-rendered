```yaml
number: 16701
title: Fix CMD path in FastAPI Dockerfile
type: pull_request
state: merged
author: jspablo
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-12T10:17:47Z
updated_at: 2025-11-16T22:57:27Z
url: https://github.com/astral-sh/uv/pull/16701
synced_at: 2026-01-10T05:58:11Z
```

# Fix CMD path in FastAPI Dockerfile

---

_Pull request opened by @jspablo on 2025-11-12 10:17_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix suggestion for uv when used with FastAPI and Docker.

## Test Plan

The Dockerfile sets `/app` as workdir, later the CMD command points again to `app/main.py` instead of only `main.py`, causing the following error:
```
Path does not exist app/main.py
```


---

_@zanieb approved on 2025-11-12 13:38_

---

_Label `documentation` added by @zanieb on 2025-11-12 13:39_

---

_Merged by @zanieb on 2025-11-12 13:39_

---

_Closed by @zanieb on 2025-11-12 13:39_

---

_Comment by @Vminoz on 2025-11-16 11:10_

@jspablo sorry, this change was incorrect, please see https://github.com/astral-sh/uv-fastapi-example

It may be somewhat confusing though that the project structure uses `app/` as main module and copies files to `/app` as per docker conventions.

In short main.py is in fact at `/app/app/main.py` at runtime and this change is invalid.

### Example
```sh
$ git clone https://github.com/astral-sh/uv-fastapi-example && cd uv-fastapi-example
Cloning into 'uv-fastapi-example'...
remote: Enumerating objects: 33, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 33 (delta 8), reused 6 (delta 6), pack-reused 14 (from 1)
Receiving objects: 100% (33/33), 26.33 KiB | 4.39 MiB/s, done.
Resolving deltas: 100% (9/9), done.
$ git rev-parse HEAD
a1e313147b9be055167fe500d457d7c0b2f51318
$ grep -e 'app\b' Dockerfile
COPY . /app
WORKDIR /app
CMD ["/app/.venv/bin/fastapi", "run", "app/main.py", "--port", "80"]
$ docker build -qt fastapi-app . && docker run -p 8000:80 fastapi-app
sha256:9c0fe8617c2c026bd7193ce1a096dc44b62180c52a8531fa7aa0e847242c1b2a
INFO     Using path app/main.py
INFO     Resolved absolute path /app/app/main.py
INFO     Searching for package file structure from directories with __init__.py
         files
INFO     Importing from /app

 ╭─ Python package file structure ─╮
 │                                 │
 │  📁 app                         │
 │  ├── 🐍 __init__.py             │
 │  └── 🐍 main.py                 │
 │                                 │
 ╰─────────────────────────────────╯

INFO     Importing module app.main
INFO     Found importable FastAPI app

 ╭── Importable FastAPI app ──╮
 │                            │
 │  from app.main import app  │
 │                            │
 ╰────────────────────────────╯

INFO     Using import string app.main:app

 ╭─────────── FastAPI CLI - Production mode ───────────╮
 │                                                     │
 │  Serving at: http://0.0.0.0:80                      │
 │                                                     │
 │  API docs: http://0.0.0.0:80/docs                   │
 │                                                     │
 │  Running in production mode, for development use:   │
 │                                                     │
 │  fastapi dev                                        │
 │                                                     │
 ╰─────────────────────────────────────────────────────╯

INFO:     Started server process [1]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:80 (Press CTRL+C to quit)
```

I suppose a reverting PR is in order or some other change to lower the risk of confusion.

---

_Comment by @zanieb on 2025-11-16 16:33_

@jspablo did you not test the change as described in the test plan.. ?

---

_Comment by @jspablo on 2025-11-16 21:35_

It was a mistake on my side when moving to a compose file, the build context was different and thought the error was the folder, which caused the misunderstanding and fixing it locally with the wrong setup.

---

_Comment by @zanieb on 2025-11-16 22:57_

Interesting, thanks for the context

---

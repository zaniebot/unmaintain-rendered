```yaml
number: 13891
title: Fix incorrect run instructions in fastapi integration guide
type: pull_request
state: closed
author: scottstephens
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-06-06T19:24:58Z
updated_at: 2025-06-06T20:01:39Z
url: https://github.com/astral-sh/uv/pull/13891
synced_at: 2026-01-10T11:10:42Z
```

# Fix incorrect run instructions in fastapi integration guide

---

_Pull request opened by @scottstephens on 2025-06-06 19:24_

## Summary

The documentation describing how to run a fastapi project in the uv/fastapi integration guide is incorrect. The existing directions result in the error "Failed to canonicalize script path". This change updates the documentation to add the `-m` flag to the `uv run` command , which is necessary to run a module from a uv project dependency.

## Test Plan

The CI build should ensure that documentation still builds properly. This is a documentation-only change.


---

_Comment by @zanieb on 2025-06-06 19:26_

Did you not get the FastAPI CLI when using `fastapi[standard]` as the dependency?

---

_Comment by @scottstephens on 2025-06-06 19:32_

@zanieb Nope, this is what I have as my dependency: `fastapi[standard]>=0.115.11`, and it works with `uv run -m fastapi dev`, but not with `uv run fastapi dev`.

---

_Comment by @zanieb on 2025-06-06 19:33_

```
❯ uv venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install 'fastapi[standard]>=0.115.11'
Resolved 35 packages in 347ms
Installed 35 packages in 51ms
 + annotated-types==0.7.0
 + anyio==4.9.0
 + certifi==2025.4.26
 + click==8.2.1
 + dnspython==2.7.0
 + email-validator==2.2.0
 + fastapi==0.115.12
 + fastapi-cli==0.0.7
 + h11==0.16.0
 + httpcore==1.0.9
 + httptools==0.6.4
 + httpx==0.28.1
 + idna==3.10
 + jinja2==3.1.6
 + markdown-it-py==3.0.0
 + markupsafe==3.0.2
 + mdurl==0.1.2
 + pydantic==2.11.5
 + pydantic-core==2.33.2
 + pygments==2.19.1
 + python-dotenv==1.1.0
 + python-multipart==0.0.20
 + pyyaml==6.0.2
 + rich==14.0.0
 + rich-toolkit==0.14.7
 + shellingham==1.5.4
 + sniffio==1.3.1
 + starlette==0.46.2
 + typer==0.16.0
 + typing-extensions==4.14.0
 + typing-inspection==0.4.1
 + uvicorn==0.34.3
 + uvloop==0.21.0
 + watchfiles==1.0.5
 + websockets==15.0.1
❯ uv run fastapi
Usage: fastapi [OPTIONS] COMMAND [ARGS]...
Try 'fastapi --help' for help.
╭─ Error ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Missing command.                                                                                                                                                                                                                          │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
❯ uv run fastapi dev

   FastAPI   Starting development server 🚀
 
             Searching for package file structure from directories with __init__.py files
 
             Could not find a default file to run, please provide an explicit path
```

---

_Comment by @zanieb on 2025-06-06 19:34_

That seems weird?

---

_Comment by @scottstephens on 2025-06-06 19:52_

Hm, tried a minimal repro closer to my actual project's setup, and `uv run fastapi dev` worked there.

Seems there must be something going on with my real project specifically, or some nuance of it I'm not capturing in the minimal repro.


```
scott@msi-scott MINGW64 ~/dev/repros/uv-fastapi
$ uv init
Initialized project `uv-fastapi`

scott@msi-scott MINGW64 ~/dev/repros/uv-fastapi (main)
$ uv add fastapi[standard]
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 37 packages in 1.08s
Prepared 35 packages in 1.50s
Installed 35 packages in 239ms
<snip>

scott@msi-scott MINGW64 ~/dev/repros/uv-fastapi (main)
$ uv run fastapi dev

   FastAPI   Starting development server 🚀

             Searching for package file structure from directories with __init__.py files

             Could not find FastAPI app in module, try using --app

```

---

_Comment by @scottstephens on 2025-06-06 20:01_

Ugh, deleted the lock file in the real project and tried again without the `-m` and it started working.

---

_Closed by @scottstephens on 2025-06-06 20:01_

---

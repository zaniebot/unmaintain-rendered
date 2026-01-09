---
number: 15246
title: uv cannot spawn gunicorn in uv project
type: issue
state: open
author: CNOCTAVE
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-08-12T22:10:03Z
updated_at: 2025-10-23T15:52:20Z
url: https://github.com/astral-sh/uv/issues/15246
synced_at: 2026-01-07T13:12:19-06:00
---

# uv cannot spawn gunicorn in uv project

---

_Issue opened by @CNOCTAVE on 2025-08-12 22:10_

### Summary

For example, without uv, 
$ gunicorn --chdir=/data/www/octave_docs/octave_docs_search_api search_api:app -w 4 -k uvicorn.workers.UvicornWorker --bind 127.0.0.1:8001
this code runs well.
But, with uv, 
$ uv run gunicorn --chdir=/data/www/octave_docs/octave_docs_search_api search_api:app -w 4 -k uvicorn.workers.UvicornWorker --bind 127.0.0.1:8001
throws an error of "error: Failed to spawn `gunicorn` Caused by: No such file or directory"


### Platform

Fedora 42 Cloud

### Version

uv 0.8.9

### Python version

Python 3.12.3

---

_Label `bug` added by @CNOCTAVE on 2025-08-12 22:10_

---

_Comment by @CNOCTAVE on 2025-08-12 22:11_

I also tried:
$ uv run -- gunicorn --chdir=/data/www/octave_docs/octave_docs_search_api search_api:app -w 4 -k uvicorn.workers.UvicornWorker --bind 127.0.0.1:8001
this code throws the exactly same error.

---

_Comment by @zanieb on 2025-08-12 22:12_

Can you share `-vv` logs of the `uv run` invocation please?

---

_Comment by @zanieb on 2025-08-12 22:12_

This is also not trivially reproducible, e.g.,

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add gunicorn
Using CPython 3.14.0rc1 interpreter at: /Users/zb/.local/bin/python3.14
Creating virtual environment at: .venv
Resolved 3 packages in 207ms
Prepared 1 package in 110ms
Installed 2 packages in 12ms
 + gunicorn==23.0.0
 + packaging==25.0
❯ uv run gunicorn

Error: No application module specified.
```

So we will need more information about your setup.

---

_Comment by @CNOCTAVE on 2025-08-12 22:13_

<img width="1920" height="945" alt="Image" src="https://github.com/user-attachments/assets/9ab2136d-4c8a-40ab-a9d5-a17d7dc5a1fb" /> @zanieb nothing more verbose

---

_Comment by @CNOCTAVE on 2025-08-12 22:14_

> This is also not trivially reproducible, e.g.,
> 
> ```
> ❯ uv init example
> Initialized project `example` at `/Users/zb/workspace/uv/example`
> ❯ cd example
> ❯ uv add gunicorn
> Using CPython 3.14.0rc1 interpreter at: /Users/zb/.local/bin/python3.14
> Creating virtual environment at: .venv
> Resolved 3 packages in 207ms
> Prepared 1 package in 110ms
> Installed 2 packages in 12ms
>  + gunicorn==23.0.0
>  + packaging==25.0
> ❯ uv run gunicorn
> 
> Error: No application module specified.
> ```
> 
> So we will need more information about your setup.

ok.

```
# cd /data/www/octave_docs
# uv init octave_docs_search_api --python 3.12.3
# cp search_api.py ./octave_docs_search_api/
# cp requirements.txt ./octave_docs_search_api/
# cd octave_docs_search_api
# uv add -r requirements.txt
```

where the requirements.txt is:
```
fastapi[all]~=0.109.2
starlette~=0.36.3
typing_extensions~=4.9.0
pydantic~=2.5.3
python-jose[cryptography]~=3.3.0
passlib~=1.7.4
uvicorn~=0.27.0
snowflake-util~=1.0.0b6
setuptools
gunicorn
```


---

_Comment by @zanieb on 2025-08-12 22:14_

Please don't use screenshots.

You're passing `-vv` to guvicorn there instead of to `uv run`.



---

_Comment by @CNOCTAVE on 2025-08-12 22:16_

> Please don't use screenshots.
> 
> You're passing `-vv` to guvicorn there instead of to `uv run`.

im using vnc.
uv misc doesnt support
$ uv something > log.txt
I want to export log too ^_^

---

_Comment by @CNOCTAVE on 2025-08-12 22:17_

<img width="1920" height="945" alt="Image" src="https://github.com/user-attachments/assets/76d4b1e4-8d7f-45cc-9fd8-5343640cc302" />

---

_Comment by @zanieb on 2025-08-12 22:18_

What does `head .venv/bin/gunicorn` show?

---

_Comment by @CNOCTAVE on 2025-08-12 22:19_

<img width="1920" height="945" alt="Image" src="https://github.com/user-attachments/assets/d430677e-ebfc-49c9-9d58-7c39b643313a" />No such file or directory

---

_Comment by @zanieb on 2025-08-12 22:21_

What version of gunicorn is being installed? Does it persist if you `uv venv --clear`? It's weird the file is missing.

---

_Comment by @CNOCTAVE on 2025-08-12 22:24_

gunicorn==23.0.0
It persists.


---

_Comment by @zanieb on 2025-08-12 22:48_

How about `uv venv --clear && uv run --no-cache gunicorn`? Are other entry points broken? This is pretty weird. Unlikely I'll be able to diagnose without a minimal reproduction on my machine or complete logs.

---

_Comment by @niels1voo on 2025-08-22 20:27_

same issue for me on amazonlinux:2023 deployed on fargate. locally it starts up fine with docker compose. same error as OP on ECS. going to rollback uv version until one works.

---

_Label `needs-mre` added by @zanieb on 2025-08-22 22:09_

---

_Comment by @nicolaygerold on 2025-10-10 06:34_

I hit this because the shebang in the .venv/bin/gunicorn was incorrect. I moved the project to a different folder. 
rm -rf .venv && uv sync did it for the local dev (also dockerized). 
I would look into your deploy scripts whether anything could produce a false shebang and check the shebang in gunicorn. 


---

_Comment by @rubms on 2025-10-23 14:35_

I've had the same issues. It works locally but in ECS it times out. My recommendation: don't use uv inside the container, and use the system Python environment instead. I tried with `uv pip install --system` but it still failed. The only way I made it work:

```dockerfile
RUN uv export --frozen --no-hashes -o requirements.txt
RUN pip install -r requirements.txt
```

This way I have the advantages of uv locally, but in CI/CD I leverage the container Python instance and prevent the issue.

---

_Comment by @konstin on 2025-10-23 15:52_

If you have a Dockerfile that reproduces this problem, please share it!

---

---
number: 7212
title: "uv sync: empty directories are assumed to be linked to non-existent interpreters"
type: issue
state: closed
author: hynek
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-09-09T07:50:37Z
updated_at: 2024-09-19T11:52:20Z
url: https://github.com/astral-sh/uv/issues/7212
synced_at: 2026-01-10T01:24:12Z
---

# uv sync: empty directories are assumed to be linked to non-existent interpreters

---

_Issue opened by @hynek on 2024-09-09 07:50_

I've been experimenting a bit with `uv python` and Docker and it turns out it's systematically kinda awkward if you want non-root user and multi-stage.

But one behavior seems a bug to me: in an attempt to install the python into the service user (called `app`), I'm trying to create a directory `/app` as root, chown it to app and then switch to `USER app` before running sync.

However, when I now try to run `uv sync` with `UV_PROJECT_ENVIRONMENT=/app`, it reports:

```
 Ignoring existing virtual environment linked to non-existent Python interpreter: /app/bin/python3
2.391 Using Python 3.12.5
2.433 error: failed to remove directory `/app`
2.433   Caused by: Permission denied (os error 13)
```

Basically it wrongly detects that an completely empty directory is linked to `/app/bin/python3` and tries to recreate it which fails, because it's not root.

I've added an ls to be sure:

```
#15 0.087 + ls -al /app
#15 0.107 total 0
#15 0.107 drwxr-xr-x 1 app  app  0 Sep  9 09:13 .
#15 0.107 drwxr-xr-x 1 root root 8 Sep  9 09:16 ..
```

I've solved it with:

```dockerfile
ENV UV_PYTHON=3.12 \
    UV_PYTHON_DOWNLOADS=true \
    UV_PYTHON_INSTALL_DIR=/python

RUN <<EOT
uv venv --python-preference=only-managed /app
chown -R app:app /app
EOT

USER app
RUN <<EOT
cd /_lock
uv sync \
    --frozen \
    --no-dev \
    --no-install-project \
    --python-preference=only-managed
EOT
```

But that's a bit awkward. If I could use an empty directory, I wouldn't have to chown. Another option would be that uv doesn't delete the whole directory, but just the contents.

---

```
❯ uv version
uv 0.4.7 (a178051e8 2024-09-07)
```

---

_Comment by @zanieb on 2024-09-10 17:57_

> Ignoring existing virtual environment linked to non-existent Python interpreter: /app/bin/python3

Is just a bad error message assuming that the given directory either

1) Exists and is actually a virtual environment
2) Doesn't exist

We don't have a case for

3) It exists, but isn't a virtual environment

So our special case error message about the interpreter not being properly linked is being shown when really it's just that we didn't find an interpreter in the expected location for a virtual environment and therefore the directory isn't a virtual environment.

We should fix that, but why are you trying to place the virtual environment in an existing directory? So you can change the ownership of it? Why not let us create the directory?

https://github.com/astral-sh/uv/blob/a8bd0211e051223be17d62c812f3de474d1e899b/crates/uv/src/commands/project/mod.rs#L385-L390

https://github.com/astral-sh/uv/blob/64e03ad56cbdf8fb03d24bb79e0f879ae7c1cabc/crates/uv-python/src/environment.rs#L136-L137

---

_Label `error messages` added by @zanieb on 2024-09-10 17:58_

---

_Label `bug` added by @zanieb on 2024-09-10 17:58_

---

_Comment by @hynek on 2024-09-10 18:23_

I was running in a few circles until I found `UV_PYTHON_INSTALL_DIR` and the bug report significantly changed while I was experimenting. :)

In the end I found it worthwhile to report this behavior either way.

I was a bit on the track trying to build as a user, so I left it but the original reason I got on said track was that the python interpreter landed in `/root/.local/` if I ran `uv sync` as root; unreachable for the app user without further chmodding.

I guess with `UV_PYTHON_INSTALL_DIR=/pythons` it makes more sense to just build as root and chown on COPY.

---

_Comment by @zanieb on 2024-09-19 11:52_

Closed by https://github.com/astral-sh/uv/pull/7527 and #7544 

---

_Closed by @zanieb on 2024-09-19 11:52_

---

_Referenced in [Fred-Hutch-Innovation-Lab/quarto-vscode-server#1](../../Fred-Hutch-Innovation-Lab/quarto-vscode-server/issues/1.md) on 2025-09-04 19:41_

---

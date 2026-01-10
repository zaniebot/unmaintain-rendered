---
number: 15601
title: "`uv venv` fails Creating virtual environment"
type: issue
state: closed
author: paulchubatyy
labels:
  - needs-mre
assignees: []
created_at: 2025-08-30T22:30:30Z
updated_at: 2025-09-05T18:50:52Z
url: https://github.com/astral-sh/uv/issues/15601
synced_at: 2026-01-10T01:25:57Z
---

# `uv venv` fails Creating virtual environment

---

_Issue opened by @paulchubatyy on 2025-08-30 22:30_

### Summary

with the following:

```
❯ uv --version
uv 0.8.14


❯ uv venv
Using CPython 3.11.13 interpreter at: /usr/local/bin/python3
Creating virtual environment at: .venv
error: Failed to create virtual environment
  Caused by: failed to open file `/app/.venv/CACHEDIR.TAG`: No such file or directory (os error 2)
```

### Platform

Linux 6.15.11-orbstack-00539-g9885ebd8e3f4 aarch64 Linux

### Version

uv 0.8.14

### Python version

CPython 3.11.13

---

_Label `bug` added by @paulchubatyy on 2025-08-30 22:30_

---

_Comment by @charliermarsh on 2025-08-30 22:33_

Strange. This seems impossible from reading the code.

---

_Comment by @charliermarsh on 2025-08-30 22:34_

Can you provide a complete reproduction, e.g., with a Dockerfile?

---

_Label `bug` removed by @charliermarsh on 2025-08-30 22:34_

---

_Label `needs-mre` added by @charliermarsh on 2025-08-30 22:34_

---

_Comment by @paulchubatyy on 2025-08-30 22:35_

Just tried on the host mac:
```
❯ uv venv
Using CPython 3.13.3 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate.fish

❯ uv --version
uv 0.6.14 (Homebrew 2025-04-09)
```

I'll use it as a workaround for now.

---

_Comment by @paulchubatyy on 2025-08-30 22:37_

Here's my Dockerfile that I used to build the VSCode remote container:

```
FROM ghcr.io/astral-sh/uv:python3.11-alpine AS base
ARG GIT_REV=development
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
ENV GIT_REV=${GIT_REV}
RUN apk add --no-cache gettext ca-certificates \
    gcc python3-dev libc-dev linux-headers postgresql-dev libffi-dev \
    jpeg-dev zlib-dev libjpeg freetype-dev redis openssh \
    fish graphviz vim jq doas make git bash postgresql-client
WORKDIR /app

FROM base AS development
RUN adduser -u 1000 -h /home/me -G wheel -D me && \
    echo "permit nopass :wheel" >> /etc/doas.d/doas.conf && \
    mkdir -p /home/me/.vscode-server/extensions && \
    chown -R vrds /home/me/.vscode-server && \
    chown -R vrds /home/me/.vscode-server/extensions
```

---

_Comment by @paulchubatyy on 2025-08-30 23:34_

I guess it was something with the volume mount. After I recreated it everything worked.

---

_Closed by @paulchubatyy on 2025-08-30 23:34_

---

_Comment by @michealroberts on 2025-09-05 18:50_

+1 This seems to be an issue when using a bound volume mount / containerisation ... I'm not sure why it happens but it's happening on >0.8.4

The workaround is to remove your .venv file, kill any containers etc and rebuild then a fresh.

---

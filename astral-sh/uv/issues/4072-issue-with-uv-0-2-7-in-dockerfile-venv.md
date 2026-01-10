---
number: 4072
title: "Issue with `uv@0.2.7` in Dockerfile + venv"
type: issue
state: closed
author: omarish
labels:
  - bug
assignees: []
created_at: 2024-06-05T20:31:37Z
updated_at: 2024-06-05T21:35:17Z
url: https://github.com/astral-sh/uv/issues/4072
synced_at: 2026-01-10T01:23:34Z
---

# Issue with `uv@0.2.7` in Dockerfile + venv

---

_Issue opened by @omarish on 2024-06-05 20:31_

I think I've encountered an issue related to (I believe) virtualenv detection when using uv in a dockerfile. Specifically, I noticed that my company's CI started failing on all images that rely on uv.

For reference, here is the dockerfile:

```dockerfile
# syntax = docker/dockerfile:1.0-experimental

# Base image
FROM python:3.12.2 as build
RUN apt-get update && apt-get install -y build-essential curl libpq-dev
ENV VIRTUAL_ENV=/opt/venv \
  PATH="/opt/venv/bin:$PATH"

ADD https://astral.sh/uv/install.sh /install.sh
RUN chmod -R 655 /install.sh && /install.sh && rm /install.sh
COPY ./packages/waldo-db-proxy/requirements.txt .
RUN /root/.cargo/bin/uv venv /opt/venv && \
  /root/.cargo/bin/uv pip install --no-cache -r requirements.txt

# App image
FROM python:3.12.2-slim-bookworm
RUN apt-get update && apt-get install -y build-essential curl libpq-dev
COPY --from=build /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
WORKDIR /app
COPY  ./packages/waldo-db-proxy ./
EXPOSE  5432
CMD [ "python", "-m", "app", "start" ]
```

This started failing universally this morning.

I hopped in to a partial image (before this line that activates the venv `RUN /root/.cargo/bin/uv venv /opt/venv`), and found that it was running uv@0.2.7.

This image works without issue when I replace `https://astral.sh/uv/install.sh` with `https://github.com/astral-sh/uv/releases/download/0.2.6/uv-installer.sh`.

I haven't had time to further debug, but just wanted to raise this issue in case others are trying to debug.

Edit: it seems to fail at this step on 0.2.7:

```sh
 > [build 6/6] RUN /root/.cargo/bin/uv venv /opt/venv &&   /root/.cargo/bin/uv pip install --no-cache -r requirements.txt:
#19 0.100   × failed to canonicalize path `/opt/venv/bin/python3`
#19 0.100   ╰─▶ No such file or directory (os error 2)
```

---

_Renamed from "Issue with uv 0.2.7 when running in Dockerfile with virtualenv" to "Issue with `uv@0.2.7` in Dockerfile + venv" by @omarish on 2024-06-05 20:32_

---

_Comment by @charliermarsh on 2024-06-05 20:32_

FWIW, you can also use `https://astral.sh/uv/0.2.6/install.sh` as the installer link.

---

_Comment by @charliermarsh on 2024-06-05 20:33_

Can you share any more on what is failing?

---

_Comment by @charliermarsh on 2024-06-05 20:36_

Modifying the Dockerfile a bit, I eventually get:

```
 > [build 6/6] RUN /root/.cargo/bin/uv venv /opt/venv &&   /root/.cargo/bin/uv pip install --no-cache -r requirements.txt:
#19 0.100   × failed to canonicalize path `/opt/venv/bin/python3`
#19 0.100   ╰─▶ No such file or directory (os error 2)
```

Does that look familiar?

---

_Comment by @omarish on 2024-06-05 20:37_

Sorry for not posting that in the original issue. Yes, that's the issue I ran into using 0.2.7.

---

_Comment by @charliermarsh on 2024-06-05 20:37_

Okay, thanks.

---

_Label `bug` added by @charliermarsh on 2024-06-05 20:38_

---

_Referenced in [astral-sh/uv#4073](../../astral-sh/uv/pulls/4073.md) on 2024-06-05 20:48_

---

_Comment by @zanieb on 2024-06-05 20:49_

Thanks! We'll have a fix out shortly.

---

_Closed by @zanieb on 2024-06-05 20:50_

---

_Comment by @omarish on 2024-06-05 21:09_

That was fast! Thank you.

---

_Comment by @zanieb on 2024-06-05 21:35_

The release should be out now. Let us know if you run into any more problems :)

---

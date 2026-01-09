---
number: 6724
title: "uv run doesn't pass signals to children"
type: issue
state: closed
author: overfl0
labels:
  - bug
assignees: []
created_at: 2024-08-27T22:05:56Z
updated_at: 2024-11-13T13:36:08Z
url: https://github.com/astral-sh/uv/issues/6724
synced_at: 2026-01-07T13:12:17-06:00
---

# uv run doesn't pass signals to children

---

_Issue opened by @overfl0 on 2024-08-27 22:05_

When you pass `uv run` as the command in `compose.yml`, you can't gracefully stop the process by pressing Ctrl+C. I'm guessing it's because it's not passing sigterm to its children.

Steps to reproduce:

- Clone https://github.com/astral-sh/uv-docker-example
- Open `compose.yml` and add `command: uv run fastapi dev --host 0.0.0.0 src/uv_docker_example`
- `docker compose build`
- `docker compose up`
- Press Ctrl+C

You will have to wait 10 seconds until docker forcibly sigkills the process.

Then:

- remove `uv run` from the command: `command: fastapi dev --host 0.0.0.0 src/uv_docker_example`
- `docker compose up`
- Press Ctrl+C

Compose will shutdown the process almost instantaneously.

uv 0.3.3 (according to uv-docker-example's Dockerfile)

---

_Comment by @zanieb on 2024-08-27 22:09_

Thanks for the report!

---

_Label `bug` added by @zanieb on 2024-08-27 22:10_

---

_Comment by @charliermarsh on 2024-08-27 23:50_

Interesting... We do pass Ctrl+C, so I would expect this to work.

---

_Comment by @overfl0 on 2024-08-28 02:10_

According to https://hynek.me/articles/docker-signals/ the signal that docker is supposed to send you is SIGTERM (so not SIGINT, which is sent on Ctrl+C), but I'm not an expert on this so all i can do is link you to that page

---

_Comment by @overfl0 on 2024-08-28 02:19_

Although I have to add that both adding `STOPSIGNAL SIGINT` to the Dockerfile and `stop_signal: SIGINT` to compose.yml failed for me, for some reason, as well.

---

_Referenced in [astral-sh/uv#6738](../../astral-sh/uv/pulls/6738.md) on 2024-08-28 02:48_

---

_Referenced in [astral-sh/uv#7449](../../astral-sh/uv/issues/7449.md) on 2024-09-17 13:29_

---

_Referenced in [astral-sh/uv#8654](../../astral-sh/uv/issues/8654.md) on 2024-10-29 14:15_

---

_Referenced in [astral-sh/uv#8761](../../astral-sh/uv/issues/8761.md) on 2024-11-01 23:49_

---

_Referenced in [astral-sh/uv#8933](../../astral-sh/uv/pulls/8933.md) on 2024-11-08 11:45_

---

_Closed by @zanieb on 2024-11-12 02:48_

---

_Closed by @zanieb on 2024-11-12 02:48_

---

_Comment by @shaneikennedy on 2024-11-13 13:36_

Would it be possible to get a new patch release with this issue now close, please? Not properly handling  SIGTERM is negatively impacting anyone running uv in production with containers. For those who know it's a problem in their workloads they could mitigate this by running uv with something like [tini](https://github.com/krallin/tini) but would prefer to get a new version here if possible 🙏 

---

_Referenced in [astral-sh/uv#12108](../../astral-sh/uv/issues/12108.md) on 2025-03-11 01:27_

---

_Referenced in [astral-sh/uv#13756](../../astral-sh/uv/issues/13756.md) on 2025-06-01 05:25_

---

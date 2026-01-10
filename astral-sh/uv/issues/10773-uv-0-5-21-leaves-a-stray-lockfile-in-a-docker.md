---
number: 10773
title: "uv 0.5.21 leaves a stray lockfile in a Docker image's /tmp"
type: issue
state: open
author: akx
labels:
  - needs-design
assignees: []
created_at: 2025-01-20T08:48:45Z
updated_at: 2025-03-17T15:13:54Z
url: https://github.com/astral-sh/uv/issues/10773
synced_at: 2026-01-10T01:24:57Z
---

# uv 0.5.21 leaves a stray lockfile in a Docker image's /tmp

---

_Issue opened by @akx on 2025-01-20 08:48_

## Environment

* uv 0.5.21 via `ghcr.io/astral-sh/uv:python3.13-bookworm-slim` (`ghcr.io/astral-sh/uv@sha256:9cc15585b36e8d7e3a9e2e43ba52b270de73722679f440fd3a5946d431a6e0e7`)
* run on Apple Silicon, Rosetta AMD64 container emulation

## Repro steps

### Dockerfile

```
FROM ghcr.io/astral-sh/uv:python3.13-bookworm-slim AS build
WORKDIR /foo
RUN uv init
RUN uv build --wheel

FROM ghcr.io/astral-sh/uv:python3.13-bookworm-slim
WORKDIR /app
RUN --mount=from=build,source=/foo/dist,target=/wheels uv --no-cache-dir pip install --system /wheels/*.whl
```

### Procedure

```
$ docker build --platform=linux/amd64 -t xxx .
$ docker run -it xxx ls -la /tmp
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
total 8
drwxrwxrwt 1 root root 4096 Jan 20 08:44 .
drwxr-xr-x 1 root root 4096 Jan 20 08:44 ..
-rw-r--r-- 1 root root    0 Jan 20 08:44 uv-1b696e695b7c17a7.lock
```

## Workaround

```
RUN --mount=from=build,source=/foo/dist,target=/wheels --mount=type=tmpfs,target=/tmp ...
```

so an ephemeral `/tmp` can't end up in the captured layer.

## Desired result

No extra files left behind in Docker image (each extra file has a measurable cost when extracting Docker images onto host systems; #7696 was related).

---

_Comment by @konstin on 2025-01-20 14:42_

We're using this file to lock the site-packages directory to avoid two uv processes changing files at the same time and creating a broken venv.

I don't know the behavior of removing a locked file cross-platform (is it safe? are there TOCTOU bugs?) which we need to figure out, CC @BurntSushi.

---

_Comment by @zanieb on 2025-01-20 17:17_

I would recommend mounting `/tmp` as a [tmpfs](https://docs.docker.com/engine/storage/tmpfs/) if this is a problem for you.

edit: I see you've suggested this a workaround — why is this not a reasonable solution?

---

_Comment by @charliermarsh on 2025-01-21 00:51_

It'd be nice to clean these up, though also unsure what's actually safe here.

---

_Label `needs-design` added by @charliermarsh on 2025-01-21 00:51_

---

_Comment by @BurntSushi on 2025-01-21 13:22_

> I don't know the behavior of removing a locked file cross-platform (is it safe? are there TOCTOU bugs?) which we need to figure out, CC @BurntSushi.

I'm unsure of the behavior either. But if you delete the lock file while the lock isn't held, then that certainly seems like it could lead to a synchronization bug. So that leaves deleting it while the lock is held, which I'm not sure is possible, and I think would require relying on anything that is _waiting_ on the locks to stop with an error. I'm not sure if that's the behavior or not.

Given that I don't know the platform behavior here, I can't immediately think of a way to safely delete these files. I think a user can if they can assert that there aren't any `uv` processes running.

Otherwise, I agree with @zanieb here. Mount `/tmp` as a `tmpfs`. `/tmp` is often treated as an ephemeral directory whose contents will eventually be removed, and thus precise clean-up isn't nearly as important.

---

_Comment by @akx on 2025-01-21 15:25_

> I see you've suggested [mounting tmp as tmpfs] as a workaround — why is this not a reasonable solution?

It's a solution, sure. That just doesn't happen by default in Docker builds, so this stray file (that doesn't appear with vanilla `pip` – but then again I suppose it doesn't play nice in concurrent situations either) caught me by surprise.

Might be at least worth documenting on the [uv Docker page](https://docs.astral.sh/uv/guides/integration/docker/)? 



---

_Comment by @zanieb on 2025-01-21 15:28_

Yeah I can't think of a way to safely delete the file either — we've considered it before.

I'm happy to review a pull request adding it to the page.

---

_Referenced in [astral-sh/uv#11178](../../astral-sh/uv/pulls/11178.md) on 2025-02-03 09:45_

---

_Referenced in [astral-sh/uv#11408](../../astral-sh/uv/issues/11408.md) on 2025-02-10 21:53_

---

_Referenced in [astral-sh/uv#11393](../../astral-sh/uv/issues/11393.md) on 2025-03-16 09:32_

---

_Comment by @nickurak on 2025-03-17 15:13_

My 2 cents: It seems odd that (even outside of containers) `uv` is virtually the only thing I see leaving stray files in /tmp when not running.

I run CI _outside_ of containers for one project (virtual envs are more than sufficient in this case) and  gathering dozens of /tmp lock files there over time stands out as unusual. It's a minor annoyance, to be sure.

---

_Referenced in [astral-sh/uv#14385](../../astral-sh/uv/issues/14385.md) on 2025-07-01 13:07_

---

_Referenced in [WeblateOrg/docker-base#156](../../WeblateOrg/docker-base/pulls/156.md) on 2025-07-02 08:47_

---

_Referenced in [astral-sh/uv#16769](../../astral-sh/uv/issues/16769.md) on 2025-12-01 20:19_

---

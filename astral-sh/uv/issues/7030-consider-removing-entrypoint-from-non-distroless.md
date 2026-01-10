---
number: 7030
title: "Consider removing `ENTRYPOINT` from non-distroless uv Docker images"
type: issue
state: closed
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2024-09-04T15:26:10Z
updated_at: 2024-09-04T23:55:22Z
url: https://github.com/astral-sh/uv/issues/7030
synced_at: 2026-01-10T01:24:09Z
---

# Consider removing `ENTRYPOINT` from non-distroless uv Docker images

---

_Issue opened by @zanieb on 2024-09-04 15:26_

Using `uv` as an entrypoint is surprising (it differs from the base image) and broke the example at https://github.com/astral-sh/uv-docker-example/pull/13 — I had to add:

```dockerfile
ENTRYPOINT []
```

cc @samypr100 

---

_Label `needs-decision` added by @zanieb on 2024-09-04 15:26_

---

_Comment by @samypr100 on 2024-09-04 15:43_

Agreed, we can change it (just deleting that line) in the CI yaml.

Since its our own images I thought we'd aim for a consistent entrypoint experience across all of them rather than retain the upstream entrypoint which would differ depending on the tag. You see that consistency often on other images derived from base images (e.g. redis, etc.).

---

_Comment by @zanieb on 2024-09-04 15:46_

Ah interesting. Hm. I'm not sure — it's probably not going to be common to use `uv` as the entrypoint since it's not a "service"? It makes sense in the distroless image because there's nothing else to invoke.

---

_Comment by @samypr100 on 2024-09-04 15:56_

> Ah interesting. Hm. I'm not sure — it's probably not going to be common to use `uv` as the entrypoint since it's not a "service"? It makes sense in the distroless image because there's nothing else to invoke.

It also applies CLI tools afaik. I do get the point though, normally by convention I expect tag variations offered by a specific project to default their entrypoint to their service/binary. If we want a different experience, we can do that as well easily.

There's two core use cases here
* Experience when pulling the image via docker run (no args) - first time experience
* Building using FROM

In the first case you immediately get uv rather than something else, and it delivers a nice new user experience (no surprises from parent image).

In the second case, its quite common to use multi-stage to solve if this becomes an issue.
On single stage you also get the benefit of leveraging CMD alongside the default ENTRYPOINT. For example CMD `run xyz` would end up becoming `uv run xyz` which I think also delivers a nice user experience focused on uv, rather than the upstream image's focus (as that could change over time).

---

_Comment by @samypr100 on 2024-09-04 18:23_

@zanieb For example, in the example from https://github.com/astral-sh/uv-docker-example

```Dockerfile
# Reset the entrypoint, don't invoke `uv`
ENTRYPOINT []

# Run the FastAPI application by default
# Uses `fastapi dev` to enable hot-reloading when the `watch` sync occurs
# Uses `--host 0.0.0.0` to allow access from outside the container
CMD ["fastapi", "dev", "--host", "0.0.0.0", "src/uv_docker_example"]
```

would become something like

```Dockerfile
# Run the FastAPI application by default
# Uses `fastapi dev` to enable hot-reloading when the `watch` sync occurs
# Uses `--host 0.0.0.0` to allow access from outside the container
CMD ["run", "fastapi", "dev", "--host", "0.0.0.0", "src/uv_docker_example"]
```

which showcases uv's capabilites further as you can see, but I might be biased towards uv everything 😆 

---

_Comment by @zanieb on 2024-09-04 18:33_

People are weirdly against having `uv run` in their `CMD` but yeah I mean I think that makes sense too.

---

_Referenced in [astral-sh/uv#6857](../../astral-sh/uv/pulls/6857.md) on 2024-09-04 21:00_

---

_Referenced in [astral-sh/uv#7054](../../astral-sh/uv/pulls/7054.md) on 2024-09-04 23:00_

---

_Comment by @samypr100 on 2024-09-04 23:36_

> People are weirdly against having `uv run` in their `CMD` but yeah I mean I think that makes sense too.

I think the change in #7054 would satisfy most workflows, and addresses the original concerns and still gives us consistency.

---

_Closed by @zanieb on 2024-09-04 23:55_

---

_Closed by @zanieb on 2024-09-04 23:55_

---

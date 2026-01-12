```yaml
number: 9093
title: "Docs(GitLab): add entrypoint for distroless image"
type: pull_request
state: merged
author: Tsafaras
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-11-13T17:20:55Z
updated_at: 2024-12-04T00:19:05Z
url: https://github.com/astral-sh/uv/pull/9093
synced_at: 2026-01-12T16:08:38Z
```

# Docs(GitLab): add entrypoint for distroless image

---

_@Tsafaras_

## Summary

Update the docs for the GitLab integration, to make it clear that an entrypoint has to be specified, when a distroless image is being used.

## Test Plan

Without specifying an entrypoint when using a distroless image:

<img src=https://github.com/user-attachments/assets/35499bd5-51d8-4016-b1d0-2d56956f82e6 width=500>

_It works if you either specify the entrypoint or if the image used in not a distroless one._

---

_Comment by @zanieb on 2024-11-13 17:23_

Hm I thought we removed the custom entrypoint in https://github.com/astral-sh/uv/pull/7054

---

_Comment by @Tsafaras on 2024-11-13 17:25_

This is the image I used:
```
ghcr.io/astral-sh/uv:python3.8-bookworm
```
Could this be why? My project is still using that Python version.

But, as you can see from the images I posted, `uv`'s version is latest (`0.5.1`).
Could also have something to do with GitLab and not with `uv`.

---

_Comment by @Tsafaras on 2024-11-20 19:00_

@zanieb Friendly reminder.
Let me know how you want to move on this.

---

_Comment by @samypr100 on 2024-11-21 00:56_

@Tsafaras On your first image example you're using the distroless image which is not supported in Gitlab as `sh` does not exist in it. That's actually one of the reason the non-distroless images were added 😄 

We do not have custom entrypoints on any of the non-distroless images. The distroless does and defaults to uv as it's the only binary in that container image.

e.g.

Non-Distroless

```bash
~/ $ docker inspect ghcr.io/astral-sh/uv:0.5-python3.8-bookworm | jq  -r '.[0].Config.Entrypoint'
null
```

Distroless

```bash
~/ $ docker inspect ghcr.io/astral-sh/uv:0.5 | jq  -r '.[0].Config.Entrypoint'
[
  "/uv"
]
```

---

_Comment by @Tsafaras on 2024-11-21 10:48_

@samypr100 Thank you for the response, especially for the last snippet!
I tried finding something similar as to what you can see on docker hub ([example](https://hub.docker.com/layers/library/python/3.13/images/sha256-47db274575b70d4943bb3d408cdd590b8285b5811f10c2e7167ba7f0cada9f2c)), but I didn't find it on GH's registry. 🤔 
In between tests I never considered I had changed the image.

Should I edit the PR to make this change in the relative docs then?
> If you're using a distroless image ... blah blah

---

_Comment by @samypr100 on 2024-11-21 13:01_

> Should I edit the PR to make this change in the relative docs then?

I think it's worth adding a note to the gitlab docs, thanks

---

_Comment by @Tsafaras on 2024-11-21 17:17_

@samypr100 @zanieb Turned the correction to a note. Please have a look.

---

_Label `documentation` added by @zanieb on 2024-11-21 17:22_

---

_Renamed from "Docs: missing entrypoint in gitlab integration" to "Docs(GitLab): add entrypoint for distroless image" by @Tsafaras on 2024-11-22 19:56_

---

_Comment by @Tsafaras on 2024-12-03 23:56_

@zanieb Friendly reminder.

---

_@zanieb approved on 2024-12-04 00:19_

Thanks!

---

_Merged by @zanieb on 2024-12-04 00:19_

---

_Closed by @zanieb on 2024-12-04 00:19_

---

---
number: 17203
title: "GIT is missing on ghcr.io/astral-sh/uv:alpine image"
type: issue
state: closed
author: FabianoLothor
labels:
  - enhancement
assignees: []
created_at: 2025-12-21T04:59:05Z
updated_at: 2025-12-25T04:04:01Z
url: https://github.com/astral-sh/uv/issues/17203
synced_at: 2026-01-07T13:12:19-06:00
---

# GIT is missing on ghcr.io/astral-sh/uv:alpine image

---

_Issue opened by @FabianoLothor on 2025-12-21 04:59_

### Summary

Is there any chance to add `git` by default in the docker images?

### Example

Sometimes you just want to:

```
uvx --from git+https://github.com/github/...
```

---

_Label `enhancement` added by @FabianoLothor on 2025-12-21 04:59_

---

_Comment by @zanieb on 2025-12-21 05:10_

We're deriving these from the upstream `alpine` image, so if it doesn't have `git` I'm hesitant to add it. It does sound convenient though 🤔 

---

_Comment by @FabianoLothor on 2025-12-21 05:17_

Honestly, it is not a big of a deal do it myself:

```powershell
docker run --rm -it `
  -v ${PWD}/tmp `
  -w /tmp `
  ghcr.io/astral-sh/uv:alpine `
  sh -c 'apk add git && uvx --from git+https://github.com/github/...'
```

It is just a little annoying the need of install `git`.

That said, I'm ok if you decide to not do it, you can close the thread.

---

_Comment by @charliermarsh on 2025-12-24 13:51_

I think I'd prefer not to add if it isn't in the upstream `alpine` image.

---

_Closed by @charliermarsh on 2025-12-24 13:51_

---

_Comment by @FabianoLothor on 2025-12-25 04:04_

Sounds good, it makes sense. Now thinking better it seems to be an request for `alpine`, but I doubt they didn't get this request before.

---

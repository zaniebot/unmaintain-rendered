```yaml
number: 17095
title: Docker tags pinning uv, python and alpine
type: issue
state: closed
author: ixxie
labels:
  - enhancement
assignees: []
created_at: 2025-12-12T10:31:12Z
updated_at: 2025-12-12T14:17:16Z
url: https://github.com/astral-sh/uv/issues/17095
synced_at: 2026-01-12T16:02:43Z
```

# Docker tags pinning uv, python and alpine

---

_@ixxie_

### Summary

Recently we had a CI/CD pipeline break because of an unexpected bump in the docker image's Alpine version.

So we went about pinning all our images, and noticed that `uv` images allow you to pin `uv` version (obviously), `python` version (makes sense) and `alpine` version (great!).

However, it seems that its not possible to pin all three:
- ✔️ [pin uv + alpine](https://hub.docker.com/r/astral/uv/tags?name=-alpine3&page=4)
- ✔️  [pin uv + python](https://hub.docker.com/r/astral/uv/tags?name=0.9.14-python3.13-alpine) 
- ❌  [pin python + alpine](https://hub.docker.com/r/astral/uv/tags?name=python3.13-alpine3)
- ❌  [pin uv + python + alpine](https://hub.docker.com/r/astral/uv/tags?name=python3.13-alpine3)

Maybe I'm missing something and this is secretly a big ask?

### Example

😍  `0.9.14-python3.13-alpine3.21`

---

_Label `enhancement` added by @ixxie on 2025-12-12 10:31_

---

_Renamed from "Docker tags pinning both uv and alpine" to "Docker tags pinning uv, python *and* alpine" by @ixxie on 2025-12-12 10:31_

---

_Renamed from "Docker tags pinning uv, python *and* alpine" to "Docker tags pinning uv, python and alpine" by @ixxie on 2025-12-12 10:31_

---

_Closed by @zanieb on 2025-12-12 14:17_

---

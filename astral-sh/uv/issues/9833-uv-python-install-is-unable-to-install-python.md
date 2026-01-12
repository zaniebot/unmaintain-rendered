```yaml
number: 9833
title: "`uv python install` is unable to install python older than 3.7"
type: issue
state: closed
author: jumper047
labels:
  - question
assignees: []
created_at: 2024-12-12T08:47:43Z
updated_at: 2024-12-13T07:33:54Z
url: https://github.com/astral-sh/uv/issues/9833
synced_at: 2026-01-12T16:00:00Z
```

# `uv python install` is unable to install python older than 3.7

---

_@jumper047_

I used `pyenv` before `uv` to manage python interpreters, and `pyenv` was able to install any existing version. Minimal version `uv` can install is 3.7. Is there any technical reason for that (like, don't know, it is not possible to have prebuilt 3.5 in repos), or this was made intentionally to, for example, encourage usage of the actual python versions?

---

_Renamed from "`uv python install` is unable to install old versions of python" to "`uv python install` is unable to install python older than 3.7" by @jumper047 on 2024-12-12 10:01_

---

_Comment by @zanieb on 2024-12-12 14:02_

Yeah we don't support older Python versions, they no longer get security fixes and it's not worth the effort to maintain the builds for them upstream. The Python build system changes between minor versions so the minor version range we support is relatively important from a technical maintenance perspective.

---

_Label `question` added by @zanieb on 2024-12-12 14:02_

---

_Comment by @jumper047 on 2024-12-12 21:29_

Ok, got it! Thank you for the answer! And what about versions that are already in repo? Is there sort of deletion roadmap, or will they be there forever?

---

_Comment by @zanieb on 2024-12-12 21:37_

There isn't a formal policy yet. They will probably be there for a long time, they're not much maintenance overhead. We drop versions upstream though, i.e., we don't do new builds of Python 3.8 anymore. As we improve the upstream builds, the maintenance overhead of supporting multiple distribution formats here will grow.

---

_Comment by @zanieb on 2024-12-12 21:38_

The versions are hard-coded into uv though, so they'll be stable for a given uv version.

---

_Closed by @jumper047 on 2024-12-13 07:33_

---

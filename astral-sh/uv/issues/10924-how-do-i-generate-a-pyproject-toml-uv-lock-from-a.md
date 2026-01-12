```yaml
number: 10924
title: How do I generate a pyproject.toml/uv.lock from a pip requirements.txt file?
type: issue
state: closed
author: tonydavis629
labels:
  - question
assignees: []
created_at: 2025-01-24T01:21:26Z
updated_at: 2025-05-20T16:02:22Z
url: https://github.com/astral-sh/uv/issues/10924
synced_at: 2026-01-12T16:00:24Z
```

# How do I generate a pyproject.toml/uv.lock from a pip requirements.txt file?

---

_@tonydavis629_

### Question

I have a normal pip environment with a requirements.txt. I want to update my project to use uv. I can install the requirements.txt with `uv pip sync requirements.txt`, but this does not change the pyproject.toml and uv.lock file, so I'm still forced to use the pip interface. Is there any way to generate a uv.lock from the pip interface, or something to that affect? The [docs](https://docs.astral.sh/uv/pip/compile/#locking-requirements) do not address this case.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @tonydavis629 on 2025-01-24 01:21_

---

_Comment by @tonydavis629 on 2025-01-24 01:27_

Found [this issue](https://github.com/astral-sh/uv/issues/6275), still should be documented

---

_Closed by @tonydavis629 on 2025-01-24 03:32_

---

_Comment by @turbotimon on 2025-05-20 16:02_

> Found [this issue](https://github.com/astral-sh/uv/issues/6275), still should be documented

There is currently a draft for that: #12382

(and just for reference some related issues #13300 #11492)

---

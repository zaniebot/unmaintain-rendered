```yaml
number: 11439
title: "How to require a managed python installation in `pyproject.toml`?"
type: issue
state: open
author: YouJiacheng
labels:
  - question
assignees: []
created_at: 2025-02-12T09:09:01Z
updated_at: 2025-02-12T12:37:19Z
url: https://github.com/astral-sh/uv/issues/11439
synced_at: 2026-01-12T16:00:36Z
```

# How to require a managed python installation in `pyproject.toml`?

---

_@YouJiacheng_

### Question

all uv managed python installations are shipped with `include`, but a system python installation maybe not.
this is a kinda XY problem, so alternatively: how to require the python to be `devel`.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @YouJiacheng on 2025-02-12 09:09_

---

_Comment by @FishAlchemist on 2025-02-12 12:37_

I'm not sure what your question is, but based on the title, I think you're looking for ``python-preference``.
https://docs.astral.sh/uv/reference/settings/#python-preference

---

```yaml
number: 14865
title: How do I go back? I executed uv init by mistake.
type: issue
state: closed
author: ciaoyizhen
labels:
  - question
assignees: []
created_at: 2025-07-24T06:14:25Z
updated_at: 2025-07-29T13:52:58Z
url: https://github.com/astral-sh/uv/issues/14865
synced_at: 2026-01-12T16:01:58Z
```

# How do I go back? I executed uv init by mistake.

---

_@ciaoyizhen_

### Question

I executed uv init in the wrong directory. What command should I use to rollback?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ciaoyizhen on 2025-07-24 06:14_

---

_Comment by @powercoconola on 2025-07-29 13:46_

All `uv init` does is create various files. To 'rollback', just delete the ones it creates.
Check the docs to see which files are created, and delete them.

https://docs.astral.sh/uv/guides/projects/

---

_Comment by @ciaoyizhen on 2025-07-29 13:52_

@powercoconola  thanks.

---

_Closed by @ciaoyizhen on 2025-07-29 13:52_

---

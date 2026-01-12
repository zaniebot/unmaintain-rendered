```yaml
number: 15691
title: Permission denied (os error 13)
type: issue
state: closed
author: indrabasak
labels:
  - bug
  - question
assignees: []
created_at: 2025-09-04T22:51:20Z
updated_at: 2025-09-05T00:19:04Z
url: https://github.com/astral-sh/uv/issues/15691
synced_at: 2026-01-12T16:02:15Z
```

# Permission denied (os error 13)

---

_@indrabasak_

### Summary

I'm encountering a permission error when trying to execute a simple uvx command. 
`uvx pycowsay 'hello world!'`

Here is the error:
```
error: failed to create directory `/Users/doej/.local/share/uv/tools`: Permission denied (os error 13)
```

### Platform

macOS 15.5

### Version

uv 0.8.15 (Homebrew 2025-09-03)

### Python version

Python 3.13.1

---

_Label `bug` added by @indrabasak on 2025-09-04 22:51_

---

_Comment by @zanieb on 2025-09-04 22:59_

It sounds like you don't have permission to use that directory? We check for installed tools there.

---

_Label `question` added by @zanieb on 2025-09-04 22:59_

---

_Comment by @indrabasak on 2025-09-05 00:19_

Yes, you are right! My `/Users/doej/.local/` directory was somehow created with root as the owner. It started working once I changed the directory ownership.

---

_Closed by @indrabasak on 2025-09-05 00:19_

---

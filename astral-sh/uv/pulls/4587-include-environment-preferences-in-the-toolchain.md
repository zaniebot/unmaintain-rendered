```yaml
number: 4587
title: Include environment preferences in the toolchain not found errors
type: pull_request
state: closed
author: zanieb
labels:
  - error messages
assignees: []
draft: true
base: main
head: zb/interpreter-missing-error
created_at: 2024-06-27T13:41:09Z
updated_at: 2024-06-27T18:23:09Z
url: https://github.com/astral-sh/uv/pull/4587
synced_at: 2026-01-10T13:48:28Z
```

# Include environment preferences in the toolchain not found errors

---

_Pull request opened by @zanieb on 2024-06-27 13:41_

This regressed as part of #4416 

```
❯ uv --version
uv 0.2.13 (fa6ed3410 2024-06-18)
❯ uv pip sync requirements.txt
error: No Python interpreters found in virtual environments
❯ uv self update
info: Checking for updates...
success: Upgraded `uv` to v0.2.17! https://github.com/astral-sh/uv/releases/tag/0.2.17
❯ uv pip sync requirements.txt
error: No Python interpreters found in system toolchains
```

Now we'll say

```
error: No Python interpreters found in virtual environments or system toolchains
```

I'm going to follow-up with special cases for `PythonEnvironment::find`


---

_Label `error messages` added by @zanieb on 2024-06-27 13:41_

---

_Closed by @zanieb on 2024-06-27 18:23_

---

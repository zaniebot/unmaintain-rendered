```yaml
number: 7272
title: Display the Python version in malformed installation key traces
type: pull_request
state: closed
author: zanieb
labels:
  - tracing
assignees: []
draft: true
base: main
head: zb/display-err
created_at: 2024-09-10T19:57:10Z
updated_at: 2024-09-10T21:18:52Z
url: https://github.com/astral-sh/uv/pull/7272
synced_at: 2026-01-10T12:53:43Z
```

# Display the Python version in malformed installation key traces

---

_Pull request opened by @zanieb on 2024-09-10 19:57_

e.g., the existing message is bad?

```
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cpython-3.13.0rc2-macos-aarch64-none`: invalid Python version: invalid digit found in string

---

_Label `tracing` added by @zanieb on 2024-09-10 19:57_

---

_Converted to draft by @zanieb on 2024-09-10 19:57_

---

_Comment by @zanieb on 2024-09-10 19:58_

I think this may be moot now that we changed the parsing in #7263 

---

_Closed by @zanieb on 2024-09-10 21:18_

---

---
number: 16997
title: "BUG: unhelpful error message when resolution fails due to a dependency cycle involving the project (with a broken version number)"
type: issue
state: open
author: neutrinoceros
labels:
  - bug
  - error messages
assignees: []
created_at: 2025-12-05T10:50:13Z
updated_at: 2025-12-05T14:38:33Z
url: https://github.com/astral-sh/uv/issues/16997
synced_at: 2026-01-07T13:12:19-06:00
---

# BUG: unhelpful error message when resolution fails due to a dependency cycle involving the project (with a broken version number)

---

_Issue opened by @neutrinoceros on 2025-12-05 10:50_

### Summary

This is a follow up to #16978, where I initially got confused over a "horrible" error message (@zanieb's words, not mine 😄). I've now minimized the reproducer into the following repo,  and included as much context as I could think of in the README. Let me know if I left anything unclear !

https://github.com/neutrinoceros/mre-astral-sh-uv-16978

### Platform

Darwin 24.6.0 arm64 (also seen on Ubuntu 24.04 in CI)

### Version

uv 0.9.15 (5eafae332 2025-12-02)

### Python version

3.14.0

---

_Label `bug` added by @neutrinoceros on 2025-12-05 10:50_

---

_Comment by @zanieb on 2025-12-05 14:38_

Thanks!

---

_Label `error messages` added by @zanieb on 2025-12-05 14:38_

---

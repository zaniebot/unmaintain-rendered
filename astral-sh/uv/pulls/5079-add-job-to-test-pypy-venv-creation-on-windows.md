```yaml
number: 5079
title: Add job to test PyPy venv creation on Windows
type: pull_request
state: merged
author: silvanocerza
labels:
  - testing
assignees: []
merged: true
base: main
head: test-pypy-windows
created_at: 2024-07-15T18:24:03Z
updated_at: 2024-07-15T21:20:19Z
url: https://github.com/astral-sh/uv/pull/5079
synced_at: 2026-01-12T16:06:37Z
```

# Add job to test PyPy venv creation on Windows

---

_@silvanocerza_

## Summary

This PR adds a new job to test that PyPy venvs are correctly created on Windows and they contain all the expected binaries.

## Test Plan

Just let CI run.


---

_Assigned to @zanieb by @zanieb on 2024-07-15 18:29_

---

_Comment by @silvanocerza on 2024-07-15 18:38_

I didn't expect a stack overflow. 😅 

---

_Comment by @zanieb on 2024-07-15 18:48_

See https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#testing-on-windows (which is handled in all of our Windows CI)

---

_Comment by @silvanocerza on 2024-07-15 19:00_

That was easy, thanks. 🙏 

I should have read the whole `CONTRIBUTING.md`. 😓 

---

_@zanieb reviewed on 2024-07-15 20:30_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:655 on 2024-07-15 20:30_

Could you add a couple of these that invoke the non- "python" copies of the executable (and for Linux too)

---

_@zanieb approved on 2024-07-15 20:30_

---

_Comment by @zanieb on 2024-07-15 20:30_

Nice work.

---

_Label `testing` added by @zanieb on 2024-07-15 20:30_

---

_Merged by @zanieb on 2024-07-15 21:20_

---

_Closed by @zanieb on 2024-07-15 21:20_

---

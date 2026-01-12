```yaml
number: 16214
title: fix recompiling every time in uv-python
type: pull_request
state: merged
author: dead10ck
labels:
  - internal
assignees: []
merged: true
base: main
head: fix/uv-python-build
created_at: 2025-10-09T19:02:12Z
updated_at: 2025-10-09T21:10:09Z
url: https://github.com/astral-sh/uv/pull/16214
synced_at: 2026-01-12T16:12:10Z
```

# fix recompiling every time in uv-python

---

_@dead10ck_

Cargo is currently recompiling uv-python on every invocation because the minified JSON output file is getting a mod time newer than the last run. Instead, set the mod time of the output file to the same as the input file.

---

_@zanieb approved on 2025-10-09 21:09_

This seems sensible to me

---

_Label `internal` added by @zanieb on 2025-10-09 21:09_

---

_Comment by @zanieb on 2025-10-09 21:10_

Thanks for following up!

---

_Merged by @zanieb on 2025-10-09 21:10_

---

_Closed by @zanieb on 2025-10-09 21:10_

---

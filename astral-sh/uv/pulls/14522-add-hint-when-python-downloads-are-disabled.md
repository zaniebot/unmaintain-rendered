```yaml
number: 14522
title: Add hint when Python downloads are disabled
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/skip-extensive
created_at: 2025-07-09T15:00:20Z
updated_at: 2025-07-10T17:06:26Z
url: https://github.com/astral-sh/uv/pull/14522
synced_at: 2026-01-10T06:53:02Z
```

# Add hint when Python downloads are disabled

---

_Pull request opened by @zanieb on 2025-07-09 15:00_

Follow-up to https://github.com/astral-sh/uv/pull/14509 to provide the _reason_ downloads are disabled and surface it as a hint rather than a debug log.

e.g.,

```
❯ cargo run -q -- run --no-managed-python -p 3.13.4 python
error: No interpreter found for Python 3.13.4 in virtual environments or search path

hint: A managed Python download is available for Python 3.13.4, but the Python preference is set to 'only system'
```

---

_Label `error messages` added by @zanieb on 2025-07-09 15:00_

---

_Marked ready for review by @zanieb on 2025-07-09 20:53_

---

_@jtfmumm reviewed on 2025-07-10 12:49_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_pin.rs`:742 on 2025-07-10 12:49_

Is it possible to also add tests for the `PythonDownloads::Manual` and `PythonPreference::OnlySystem` hints?

---

_@jtfmumm approved on 2025-07-10 12:50_

---

_Merged by @zanieb on 2025-07-10 17:06_

---

_Closed by @zanieb on 2025-07-10 17:06_

---

_Branch deleted on 2025-07-10 17:06_

---

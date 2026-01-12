```yaml
number: 13922
title: "Warn if a directory specified in `--index` exists but is empty"
type: issue
state: closed
author: jtfmumm
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2025-06-09T16:21:23Z
updated_at: 2025-06-18T08:48:22Z
url: https://github.com/astral-sh/uv/issues/13922
synced_at: 2026-01-12T16:01:39Z
```

# Warn if a directory specified in `--index` exists but is empty

---

_@jtfmumm_

Currently, if a directory specified by `--index` for `uv add` is empty, uv will fall back to PyPI without warning the user. This is likely not what was intended so we should add a warning. 

---

_Label `error messages` added by @jtfmumm on 2025-06-09 16:21_

---

_Label `good first issue` added by @zanieb on 2025-06-09 16:28_

---

_Comment by @aaron-ang on 2025-06-10 04:16_

Hi, I'd like to take a stab at this.

---

_Assigned to @aaron-ang by @konstin on 2025-06-10 09:44_

---

_Assigned to @jtfmumm by @konstin on 2025-06-10 09:44_

---

_Unassigned @jtfmumm by @jtfmumm on 2025-06-10 17:18_

---

_Closed by @jtfmumm on 2025-06-18 08:48_

---

```yaml
number: 13852
title: PEP 751 uv export supports --no-editable
type: pull_request
state: merged
author: samypr100
labels:
  - bug
assignees: []
merged: true
base: main
head: pylock-format-no-editable
created_at: 2025-06-05T03:01:32Z
updated_at: 2025-06-06T15:00:57Z
url: https://github.com/astral-sh/uv/pull/13852
synced_at: 2026-01-12T16:10:54Z
```

# PEP 751 uv export supports --no-editable

---

_@samypr100_

## Summary

When trying out `uv export --no-editable --format pylock.toml` the exported contents would still retain `editable = true` regardless.

## Test Plan

Added additional test. Tested locally on few projects where I was previously using `uv export --no-editable --format requirements.txt` to ensure the output aligns.


---

_Marked ready for review by @samypr100 on 2025-06-05 03:19_

---

_Label `enhancement` added by @konstin on 2025-06-05 12:18_

---

_Label `enhancement` removed by @konstin on 2025-06-05 12:21_

---

_Label `bug` added by @konstin on 2025-06-05 12:21_

---

_@konstin approved on 2025-06-05 12:37_

Thanks! 

---

_Merged by @charliermarsh on 2025-06-06 11:52_

---

_Closed by @charliermarsh on 2025-06-06 11:52_

---

_Branch deleted on 2025-06-06 15:00_

---

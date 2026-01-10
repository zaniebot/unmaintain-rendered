```yaml
number: 12278
title: "Allow owned string when deserializing `requires-python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cow
created_at: 2025-03-18T13:09:04Z
updated_at: 2025-03-18T13:27:23Z
url: https://github.com/astral-sh/uv/pull/12278
synced_at: 2026-01-10T11:10:39Z
```

# Allow owned string when deserializing `requires-python`

---

_Pull request opened by @charliermarsh on 2025-03-18 13:09_

## Summary

I suspect this only affects packages with quotes in the requires-python, which is typically an error but one that we correct for in `LenientVersionSpecifiers`.

Closes https://github.com/astral-sh/uv/issues/12260.


---

_Label `bug` added by @charliermarsh on 2025-03-18 13:09_

---

_@Gankra approved on 2025-03-18 13:10_

classic serde_json trap, alas

---

_@konstin approved on 2025-03-18 13:16_

---

_Merged by @charliermarsh on 2025-03-18 13:27_

---

_Closed by @charliermarsh on 2025-03-18 13:27_

---

_Branch deleted on 2025-03-18 13:27_

---

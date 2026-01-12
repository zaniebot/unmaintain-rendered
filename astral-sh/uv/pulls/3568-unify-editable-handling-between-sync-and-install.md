```yaml
number: 3568
title: "Unify editable handling between `sync` and `install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/built-editable
created_at: 2024-05-14T02:46:16Z
updated_at: 2024-05-14T09:18:30Z
url: https://github.com/astral-sh/uv/pull/3568
synced_at: 2026-01-12T16:05:43Z
```

# Unify editable handling between `sync` and `install`

---

_@charliermarsh_

## Summary

Uses the editable handling from `pip sync`, and improves the abstractions such that we can pass those resolved editables into the resolver.


---

_Review requested from @konstin by @charliermarsh on 2024-05-14 02:49_

---

_Label `internal` added by @charliermarsh on 2024-05-14 02:49_

---

_Marked ready for review by @charliermarsh on 2024-05-14 02:49_

---

_Comment by @charliermarsh on 2024-05-14 03:00_

I'm so annoyed that this _adds_ six lines lol, there's so much deduplication in here.

---

_Comment by @charliermarsh on 2024-05-14 03:04_

@konstin - Feel free to merge this in the morning if you approve.

---

_@konstin approved on 2024-05-14 09:12_

---

_Merged by @konstin on 2024-05-14 09:18_

---

_Closed by @konstin on 2024-05-14 09:18_

---

_Branch deleted on 2024-05-14 09:18_

---

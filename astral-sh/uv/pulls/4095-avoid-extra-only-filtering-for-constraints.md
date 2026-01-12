```yaml
number: 4095
title: Avoid extra-only filtering for constraints
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-06-06T13:50:54Z
updated_at: 2024-06-06T14:32:15Z
url: https://github.com/astral-sh/uv/pull/4095
synced_at: 2026-01-12T16:06:02Z
```

# Avoid extra-only filtering for constraints

---

_@charliermarsh_

## Summary

The "only include if relevant for the extra" filtering should _not_ be applied to constraints. Otherwise, we'd only constrain when the extra was included in the constraints file itself, which is incorrect.

Closes https://github.com/astral-sh/uv/issues/4091.


---

_Label `bug` added by @charliermarsh on 2024-06-06 13:51_

---

_Marked ready for review by @charliermarsh on 2024-06-06 13:51_

---

_Merged by @charliermarsh on 2024-06-06 13:58_

---

_Closed by @charliermarsh on 2024-06-06 13:58_

---

_Branch deleted on 2024-06-06 13:58_

---

_Comment by @zanieb on 2024-06-06 14:31_

Wow sneaky.

---

_Comment by @charliermarsh on 2024-06-06 14:32_

I wasn't sure if that was right or not when I added it. It was confusing me... but now it makes sense.

---

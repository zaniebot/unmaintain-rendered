```yaml
number: 844
title: Show available pre-releases in error hints
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/show
created_at: 2024-01-09T03:03:01Z
updated_at: 2024-01-09T14:59:29Z
url: https://github.com/astral-sh/uv/pull/844
synced_at: 2026-01-10T15:44:44Z
```

# Show available pre-releases in error hints

---

_Pull request opened by @charliermarsh on 2024-01-09 03:03_

## Summary

If pre-releases are available for a package that we otherwise couldn't resolve, we now show a hint that includes one of the example versions.

Closes https://github.com/astral-sh/puffin/issues/811.

---

_Marked ready for review by @charliermarsh on 2024-01-09 03:04_

---

_@konstin approved on 2024-01-09 09:37_

---

_@zanieb approved on 2024-01-09 14:53_

Sweet! 

---

_Merged by @charliermarsh on 2024-01-09 14:58_

---

_Closed by @charliermarsh on 2024-01-09 14:58_

---

_Branch deleted on 2024-01-09 14:58_

---

_Label `error messages` added by @charliermarsh on 2024-01-09 14:58_

---

_Comment by @charliermarsh on 2024-01-09 14:59_

We could probably use this for yanked dependencies too and other cases?

---

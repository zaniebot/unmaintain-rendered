```yaml
number: 2420
title: "Remove `django` as a common test package"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/django
created_at: 2024-03-13T18:53:06Z
updated_at: 2024-03-13T19:46:58Z
url: https://github.com/astral-sh/uv/pull/2420
synced_at: 2026-01-12T16:05:02Z
```

# Remove `django` as a common test package

---

_@charliermarsh_

## Summary

Django is actually pretty large (the wheel is 8MB, the source distribution is 10MB). There's nothing specific to Django in any of these tests, so this just replaces it with a much smaller dependency.

We should prune these down eventually since the scenarios cover a lot of this -- this is just a bandaid.

---

_Label `testing` added by @charliermarsh on 2024-03-13 18:53_

---

_@zanieb approved on 2024-03-13 18:54_

---

_@konstin approved on 2024-03-13 18:58_

---

_Marked ready for review by @charliermarsh on 2024-03-13 19:46_

---

_Merged by @charliermarsh on 2024-03-13 19:46_

---

_Closed by @charliermarsh on 2024-03-13 19:46_

---

_Branch deleted on 2024-03-13 19:46_

---

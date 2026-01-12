```yaml
number: 3826
title: Ignore unnamed requirements in preferences
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unnamed
created_at: 2024-05-24T17:24:20Z
updated_at: 2024-05-24T17:32:21Z
url: https://github.com/astral-sh/uv/pull/3826
synced_at: 2026-01-12T16:05:52Z
```

# Ignore unnamed requirements in preferences

---

_@charliermarsh_

## Summary

We actually _already_ ignore these (preferences only apply to versions, not URLs), it just happens later on. This PR thus just avoids crashing. The behavior is unchanged.

Closes #3822.


---

_Label `bug` added by @charliermarsh on 2024-05-24 17:24_

---

_@konstin approved on 2024-05-24 17:26_

---

_Comment by @charliermarsh on 2024-05-24 17:27_

I'm going to refactor the types, clean this up and make it more explicit -- but this is fine for now.

---

_Merged by @charliermarsh on 2024-05-24 17:32_

---

_Closed by @charliermarsh on 2024-05-24 17:32_

---

_Branch deleted on 2024-05-24 17:32_

---

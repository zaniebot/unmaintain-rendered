```yaml
number: 1090
title: "Use `parse_headers` rather than parsing body"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/header
created_at: 2024-01-25T02:16:55Z
updated_at: 2024-01-25T08:41:22Z
url: https://github.com/astral-sh/uv/pull/1090
synced_at: 2026-01-10T15:39:03Z
```

# Use `parse_headers` rather than parsing body

---

_Pull request opened by @charliermarsh on 2024-01-25 02:16_

Looking at the internals, this should make almost no difference in performance, but anyway...

---

_Label `internal` added by @charliermarsh on 2024-01-25 02:16_

---

_@konstin approved on 2024-01-25 08:41_

Good idea, i think this was a leftover from when we where still parsing the body as description

---

_Merged by @konstin on 2024-01-25 08:41_

---

_Closed by @konstin on 2024-01-25 08:41_

---

_Branch deleted on 2024-01-25 08:41_

---

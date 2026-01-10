```yaml
number: 13281
title: Retry streaming downloads on broken pipe errors
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: konsti/retry-broken-pipe
created_at: 2025-05-03T23:34:29Z
updated_at: 2025-05-04T12:56:17Z
url: https://github.com/astral-sh/uv/pull/13281
synced_at: 2026-01-10T11:10:41Z
```

# Retry streaming downloads on broken pipe errors

---

_Pull request opened by @konstin on 2025-05-03 23:34_

Educated guess at #12359

See https://github.com/hyperium/h2/blob/adab70fd9f9e5ce3099d274a4b548a27bfdee4dc/src/proto/streams/state.rs#L309-L310 for the error source.

---

_Label `bug` added by @konstin on 2025-05-03 23:34_

---

_Label `network` added by @konstin on 2025-05-03 23:34_

---

_@charliermarsh approved on 2025-05-04 12:56_

---

_Merged by @charliermarsh on 2025-05-04 12:56_

---

_Closed by @charliermarsh on 2025-05-04 12:56_

---

_Branch deleted on 2025-05-04 12:56_

---

```yaml
number: 2250
title: "Stop exposing `client_raw`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/middleware
created_at: 2024-03-06T20:27:52Z
updated_at: 2024-03-06T20:37:20Z
url: https://github.com/astral-sh/uv/pull/2250
synced_at: 2026-01-12T16:04:56Z
```

# Stop exposing `client_raw`

---

_@charliermarsh_

## Summary

This is no longer necessary as `AsyncHttpRangeReader` now accepts `ClientWithMiddleware` -- which is good, because it means all relevant middleware will be enforced (like offline, or `.netrc` in the future).

---

_Label `internal` added by @charliermarsh on 2024-03-06 20:27_

---

_Marked ready for review by @charliermarsh on 2024-03-06 20:27_

---

_@zanieb approved on 2024-03-06 20:35_

---

_Merged by @charliermarsh on 2024-03-06 20:37_

---

_Closed by @charliermarsh on 2024-03-06 20:37_

---

_Branch deleted on 2024-03-06 20:37_

---

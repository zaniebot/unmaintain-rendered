```yaml
number: 16223
title: Move parsing concurrency env variable to EnvironmentOptions
type: pull_request
state: merged
author: andrey-berenda
labels:
  - internal
assignees: []
merged: true
base: main
head: move-concurrency-to-EnvironmentOptions
created_at: 2025-10-10T08:35:18Z
updated_at: 2025-10-13T01:22:35Z
url: https://github.com/astral-sh/uv/pull/16223
synced_at: 2026-01-10T06:36:15Z
```

# Move parsing concurrency env variable to EnvironmentOptions

---

_Pull request opened by @andrey-berenda on 2025-10-10 08:35_

## Summary
- Move  parsing `UV_CONCURRENT_INSTALLS`, `UV_CONCURRENT_BUILDS` and `UV_CONCURRENT_DOWNLOADS` to `EnvironmentOptions`

Relates https://github.com/astral-sh/uv/issues/14720

## Test Plan

- Tests with existing tests
- Add test with invalid parsing

---

_Renamed from "Move concurrency to EnvironmentOptions" to "Move parsing concurrency env variable to EnvironmentOptions" by @andrey-berenda on 2025-10-10 08:44_

---

_Marked ready for review by @andrey-berenda on 2025-10-10 08:51_

---

_@charliermarsh approved on 2025-10-13 01:22_

---

_Label `internal` added by @charliermarsh on 2025-10-13 01:22_

---

_Merged by @charliermarsh on 2025-10-13 01:22_

---

_Closed by @charliermarsh on 2025-10-13 01:22_

---

_Comment by @charliermarsh on 2025-10-13 01:22_

Thanks!

---

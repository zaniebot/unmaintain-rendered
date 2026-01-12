```yaml
number: 16945
title: "Temporarily drop `crates-publish` from the release for 0.9.15 re-run"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/crates-publish
created_at: 2025-12-03T00:51:45Z
updated_at: 2025-12-03T01:02:30Z
url: https://github.com/astral-sh/uv/pull/16945
synced_at: 2026-01-12T16:12:31Z
```

# Temporarily drop `crates-publish` from the release for 0.9.15 re-run

---

_@zanieb_

See https://github.com/astral-sh/uv/pull/16944

The `crates.io` publish succeeded and is not idempotent (i.e., it'll fail on another publish attempt) so we will skip it for a re-run of the release workflow.

---

_@woodruffw approved on 2025-12-03 00:53_

---

_Merged by @zanieb on 2025-12-03 00:57_

---

_Closed by @zanieb on 2025-12-03 00:57_

---

_Branch deleted on 2025-12-03 00:57_

---

_Label `internal` added by @zanieb on 2025-12-03 01:02_

---

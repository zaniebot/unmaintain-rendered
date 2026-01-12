```yaml
number: 8852
title: Use cache in windows clippy job
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: ci-cache
created_at: 2024-11-06T03:43:40Z
updated_at: 2024-11-06T05:13:49Z
url: https://github.com/astral-sh/uv/pull/8852
synced_at: 2026-01-12T16:08:31Z
```

# Use cache in windows clippy job

---

_@j178_

Place the `Swatinem/rust-cache@v2` step after `setup-dev-drive.ps1` to ensure the correct directories are cached.



---

_Comment by @j178 on 2024-11-06 04:08_

Compare https://github.com/astral-sh/uv/actions/runs/11696928991 (after this PR) with https://github.com/astral-sh/uv/actions/runs/11696831053/job/32574578634 (before this PR), we can see the `Downloaded` step in `cargo clippy` has been skipped.

---

_Marked ready for review by @j178 on 2024-11-06 04:08_

---

_@zanieb approved on 2024-11-06 05:05_

---

_Label `internal` added by @zanieb on 2024-11-06 05:05_

---

_Merged by @zanieb on 2024-11-06 05:05_

---

_Closed by @zanieb on 2024-11-06 05:05_

---

_Branch deleted on 2024-11-06 05:13_

---

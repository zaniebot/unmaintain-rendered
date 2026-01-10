```yaml
number: 3691
title: "Better error message for `uv run` failures"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: konsti/uv-run-error-messages
created_at: 2024-05-21T11:17:05Z
updated_at: 2024-05-21T11:24:19Z
url: https://github.com/astral-sh/uv/pull/3691
synced_at: 2026-01-10T14:32:20Z
```

# Better error message for `uv run` failures

---

_Pull request opened by @konstin on 2024-05-21 11:17_

Attach path context to `uv run` failures since those function calls are not covered by `fs_err`.


---

_Label `error messages` added by @konstin on 2024-05-21 11:17_

---

_@konstin reviewed on 2024-05-21 11:17_

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:171 on 2024-05-21 11:17_

This happens when the file was not found

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:172 on 2024-05-21 11:17_

I don't know when this would happen

---

_@konstin reviewed on 2024-05-21 11:17_

---

_Label `preview` added by @konstin on 2024-05-21 11:24_

---

_Merged by @konstin on 2024-05-21 11:24_

---

_Closed by @konstin on 2024-05-21 11:24_

---

_Branch deleted on 2024-05-21 11:24_

---

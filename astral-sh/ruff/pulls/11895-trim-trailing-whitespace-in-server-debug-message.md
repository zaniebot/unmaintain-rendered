```yaml
number: 11895
title: Trim trailing whitespace in server debug message
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/trailing
created_at: 2024-06-17T05:39:11Z
updated_at: 2024-06-17T05:46:09Z
url: https://github.com/astral-sh/ruff/pull/11895
synced_at: 2026-01-12T15:55:39Z
```

# Trim trailing whitespace in server debug message

---

_@dhruvmanila_

_No description provided._

---

_Label `internal` added by @dhruvmanila on 2024-06-17 05:39_

---

_@dhruvmanila reviewed on 2024-06-17 05:39_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:148 on 2024-06-17 05:39_

The `client_capabilities` uses `display_settings` macro which adds a newline at the end.

---

_Merged by @dhruvmanila on 2024-06-17 05:46_

---

_Closed by @dhruvmanila on 2024-06-17 05:46_

---

_Branch deleted on 2024-06-17 05:46_

---

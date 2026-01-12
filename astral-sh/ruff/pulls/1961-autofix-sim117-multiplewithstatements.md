```yaml
number: 1961
title: Autofix SIM117 (MultipleWithStatements)
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: autofix-sim117
created_at: 2023-01-18T15:45:09Z
updated_at: 2023-01-18T16:15:22Z
url: https://github.com/astral-sh/ruff/pull/1961
synced_at: 2026-01-12T05:25:13Z
```

# Autofix SIM117 (MultipleWithStatements)

---

_Pull request opened by @andersk on 2023-01-18 15:45_

This is slightly buggy due to Instagram/LibCST#855; it will complain `[ERROR] Failed to fix nested with: Failed to extract CST from source` when trying to fix nested parenthesized `with` statements lacking trailing commas. But presumably people who write parenthesized `with` statements already knew that they don’t need to nest them.

---

_Merged by @charliermarsh on 2023-01-18 16:06_

---

_Closed by @charliermarsh on 2023-01-18 16:06_

---

_Branch deleted on 2023-01-18 16:15_

---

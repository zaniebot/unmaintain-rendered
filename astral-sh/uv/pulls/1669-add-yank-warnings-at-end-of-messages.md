```yaml
number: 1669
title: Add yank warnings at end of messages
type: pull_request
state: merged
author: dwreeves
labels:
  - error messages
assignees: []
merged: true
base: main
head: 1292-yank-warnings-at-end
created_at: 2024-02-18T23:23:06Z
updated_at: 2024-02-19T00:01:00Z
url: https://github.com/astral-sh/uv/pull/1669
synced_at: 2026-01-12T16:04:41Z
```

# Add yank warnings at end of messages

---

_@dwreeves_

Resolves #1292.

## Summary

Move the yanked warnings for `uv pip sync` and `uv pip install` to the end of the commands, as per #1292.

## Test Plan

I ran the unit tests: `cargo nextest run`

---

_@zanieb approved on 2024-02-19 00:00_

Thank you!

---

_Label `error messages` added by @zanieb on 2024-02-19 00:00_

---

_Merged by @zanieb on 2024-02-19 00:01_

---

_Closed by @zanieb on 2024-02-19 00:01_

---

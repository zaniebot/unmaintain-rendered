```yaml
number: 6473
title: "feat(self-update): show old version in update message"
type: pull_request
state: merged
author: mkniewallner
labels:
  - enhancement
  - tracing
assignees: []
merged: true
base: main
head: feat/show-old-version-self-update
created_at: 2024-08-22T23:36:23Z
updated_at: 2024-08-23T13:50:26Z
url: https://github.com/astral-sh/uv/pull/6473
synced_at: 2026-01-10T13:09:51Z
```

# feat(self-update): show old version in update message

---

_Pull request opened by @mkniewallner on 2024-08-22 23:36_

## Summary

Indicate the previous version from which uv was upgraded when running `uv self update`. Thought that it could be useful in some situations to have a trace of the previous version that was installed.

## Test Plan

Did not find a way to test this, since this heavily relies on being able to use the installation script and the ability to publish artifacts for a specific tag.

---

_Marked ready for review by @mkniewallner on 2024-08-22 23:46_

---

_Label `enhancement` added by @zanieb on 2024-08-22 23:48_

---

_Label `tracing` added by @zanieb on 2024-08-22 23:48_

---

_Comment by @zanieb on 2024-08-22 23:49_

Ah this sounds quite nice.

---

_@zanieb reviewed on 2024-08-22 23:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_update.rs`:77 on 2024-08-22 23:50_

You can use let with a pattern to avoid unwrapping, e.g., `if let Some(old_version) = result.old_version`

---

_Assigned to @zanieb by @zanieb on 2024-08-22 23:50_

---

_@mkniewallner reviewed on 2024-08-23 01:11_

---

_Review comment by @mkniewallner on `crates/uv/src/commands/self_update.rs`:77 on 2024-08-23 01:11_

I didn't know (or forgot?) that it was possible to use that while defining a variable, really nice, thanks!

---

_@zanieb approved on 2024-08-23 12:55_

---

_Merged by @zanieb on 2024-08-23 12:55_

---

_Closed by @zanieb on 2024-08-23 12:55_

---

_Branch deleted on 2024-08-23 13:05_

---

_Comment by @notatallshaw on 2024-08-23 13:45_

This fixed https://github.com/astral-sh/uv/issues/4748 right?

---

_Comment by @zanieb on 2024-08-23 13:50_

Thank you!

---

```yaml
number: 6374
title: "Add `uv init --from-project <path>`"
type: pull_request
state: open
author: blueraft
labels:
  - enhancement
assignees: []
base: main
head: init-from-project
created_at: 2024-08-21T20:07:46Z
updated_at: 2025-04-14T08:00:33Z
url: https://github.com/astral-sh/uv/pull/6374
synced_at: 2026-01-10T11:10:34Z
```

# Add `uv init --from-project <path>`

---

_Pull request opened by @blueraft on 2024-08-21 20:07_

## Summary

Resolves https://github.com/astral-sh/uv/issues/6277

## Test Plan

`cargo test`

---

_@zanieb reviewed on 2024-08-21 20:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2099 on 2024-08-21 20:16_

I'd say "`--from-project` option" for consistency

---

_Label `enhancement` added by @zanieb on 2024-08-21 20:20_

---

_Assigned to @charliermarsh by @zanieb on 2024-08-21 20:20_

---

_Comment by @zanieb on 2024-08-21 20:36_

We should probably test with a Poetry project, e.g. https://github.com/astral-sh/packse/commit/21e7d6b4f915362dca71f77659bf5826ebd6ff67 (the commit before I switched to uv!)

---

_Comment by @blueraft on 2024-08-21 22:14_

Added a test with Poetry

---

_@blueraft reviewed on 2024-09-04 12:52_

---

_Review comment by @blueraft on `crates/uv/tests/init.rs`:1588 on 2024-09-04 12:52_

The build system metadata is now gone since the `0.4` update. But I'm not sure if initialising as an application is the best default here though. 

---

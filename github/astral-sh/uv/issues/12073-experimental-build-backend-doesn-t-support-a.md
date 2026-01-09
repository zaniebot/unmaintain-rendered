---
number: 12073
title: "Experimental build backend doesn't support a `project.scripts` with , in the name"
type: issue
state: open
author: jennifgcrl
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2025-03-09T05:14:16Z
updated_at: 2025-03-10T11:26:38Z
url: https://github.com/astral-sh/uv/issues/12073
synced_at: 2026-01-07T13:12:18-06:00
---

# Experimental build backend doesn't support a `project.scripts` with , in the name

---

_Issue opened by @jennifgcrl on 2025-03-09 05:14_

### Summary

The new uv build backend doesn't support project scripts with `,` in the name. This is allowed by [the spec](https://packaging.python.org/en/latest/specifications/entry-points/#entry-points) but I see that uv is [intentionally stricter](https://github.com/astral-sh/uv/blob/main/crates/uv-build-backend/src/metadata.rs#L608). I was wondering if some consideration could be given to projects that already use entrypoints that don't follow the recommendation. Maybe there should be an override?

### Platform

all

### Version

uv 0.6.5 (bcbcd0a1e 2025-03-06)

### Python version

_No response_

---

_Label `bug` added by @jennifgcrl on 2025-03-09 05:14_

---

_Comment by @konstin on 2025-03-10 09:20_

Can you describe why the project has a comma in its script name?

---

_Comment by @jennifgcrl on 2025-03-10 09:34_

😅 It's allowed and avoids naming collisions with all other commands.

---

_Label `needs-decision` added by @konstin on 2025-03-10 11:26_

---

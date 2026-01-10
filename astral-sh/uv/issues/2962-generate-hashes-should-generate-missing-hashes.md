---
number: 2962
title: "--generate-hashes should generate missing hashes for pinned packages"
type: issue
state: closed
author: CharString
labels:
  - bug
assignees: []
created_at: 2024-04-10T14:17:34Z
updated_at: 2024-04-10T14:56:35Z
url: https://github.com/astral-sh/uv/issues/2962
synced_at: 2026-01-10T01:23:23Z
---

# --generate-hashes should generate missing hashes for pinned packages

---

_Issue opened by @CharString on 2024-04-10 14:17_

Since #2532 uv "preserves" hashes for pinned packages. This is correct, but if no hash is present for a pinned package, a hash should still be generated.

When "turning on --generate-hashes" for a project, I don't want to run with --update, but I do want to generate hashes for pinned versions. Because pip will expect all hashes to be present:

> ERROR: Hashes are required in --require-hashes mode, but they are missing from some requirements. Here is a list of those requirements along with the hashes their downloaded archives actually had. Add lines like these to your requirements files to prevent tampering. (If you did not enable --require-hashes manually, note that it turns on automatically when any package has a hash.)

Workaround: rollback to uv 0.1.22 when preservation of hashes wasn't implemented yet.

---

_Comment by @charliermarsh on 2024-04-10 14:21_

I think that makes sense. Can change it...

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 14:21_

---

_Label `bug` added by @zanieb on 2024-04-10 14:35_

---

_Referenced in [astral-sh/uv#2966](../../astral-sh/uv/pulls/2966.md) on 2024-04-10 14:45_

---

_Closed by @charliermarsh on 2024-04-10 14:56_

---

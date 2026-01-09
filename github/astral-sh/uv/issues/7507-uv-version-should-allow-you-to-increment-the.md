---
number: 7507
title: uv version should allow you to increment the project version like the poetry version command
type: issue
state: closed
author: krishan-patel001
labels:
  - duplicate
assignees: []
created_at: 2024-09-18T16:36:27Z
updated_at: 2024-09-18T16:47:46Z
url: https://github.com/astral-sh/uv/issues/7507
synced_at: 2026-01-07T13:12:17-06:00
---

# uv version should allow you to increment the project version like the poetry version command

---

_Issue opened by @krishan-patel001 on 2024-09-18 16:36_

Currently the `uv version` command only prints out the current version. This could be updated to take in an argument or major, minor or patch or a custom version to increment the version in the `pyproject.toml` similarly to how poetry does this.

If the current version was 1.0.0 in the pyproject.toml the commands would change that value as follows
- `uv version patch` -> 1.0.1
- `uv version minor` -> 1.1.0
- `uv version major` -> 2.0.0
- `uv version 1.1.1.dev101212312` -> 1.1.1.dev101212312

---

_Comment by @zanieb on 2024-09-18 16:47_

Please remember to search for existing issues https://github.com/astral-sh/uv/issues/6298

---

_Closed by @zanieb on 2024-09-18 16:47_

---

_Label `duplicate` added by @zanieb on 2024-09-18 16:47_

---

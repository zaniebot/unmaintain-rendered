---
number: 2133
title: API usage
type: issue
state: closed
author: ofek
labels:
  - question
  - rustlib
assignees: []
created_at: 2024-03-02T18:08:48Z
updated_at: 2024-04-12T20:12:24Z
url: https://github.com/astral-sh/uv/issues/2133
synced_at: 2026-01-10T01:23:13Z
---

# API usage

---

_Issue opened by @ofek on 2024-03-02 18:08_

Hello there! I would like to use the internal crates here in [PyApp](https://github.com/ofek/pyapp). As far as I understand, the only functionality that I require would be:

1. creating virtual environments using a specific path to a Python binary
2. installing packages like `uv pip install ...` (users are allowed to pass extra flags so must be CLI)

I'm assuming the first one wouldn't be too challenging but the latter might require shipping the actual CLI/everything. How would I go about this, or even should I at this time?

---

_Label `question` added by @zanieb on 2024-03-02 18:14_

---

_Label `rustlib` added by @zanieb on 2024-03-02 18:14_

---

_Comment by @charliermarsh on 2024-03-11 20:06_

Are you open to interacting with `uv` over the CLI, rather than using it as a Rust library?

---

_Comment by @ofek on 2024-03-11 20:12_

Certainly but not for this specific use case if possible. I would much prefer to import the necessary crates.

---

_Comment by @zanieb on 2024-04-12 20:12_

I don't think our Rust APIs are stable enough for this — we would rather focus on the user experience at this time.

---

_Closed by @zanieb on 2024-04-12 20:12_

---

_Referenced in [aspect-build/rules_py#345](../../aspect-build/rules_py/issues/345.md) on 2024-06-03 12:57_

---

_Referenced in [astral-sh/uv#6080](../../astral-sh/uv/issues/6080.md) on 2024-08-14 14:41_

---

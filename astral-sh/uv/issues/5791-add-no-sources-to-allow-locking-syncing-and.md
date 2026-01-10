---
number: 5791
title: "Add `--no-sources` to allow locking, syncing, and installing without `tool.uv.sources`"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
created_at: 2024-08-05T12:53:26Z
updated_at: 2024-08-06T14:14:21Z
url: https://github.com/astral-sh/uv/issues/5791
synced_at: 2026-01-10T01:23:52Z
---

# Add `--no-sources` to allow locking, syncing, and installing without `tool.uv.sources`

---

_Issue opened by @charliermarsh on 2024-08-05 12:53_

It's seems important to be able to test this workflow, if you consider sources to be dev dependencies.

---

_Label `needs-decision` added by @charliermarsh on 2024-08-05 12:53_

---

_Label `cli` added by @charliermarsh on 2024-08-05 12:53_

---

_Label `preview` added by @charliermarsh on 2024-08-05 12:53_

---

_Comment by @charliermarsh on 2024-08-05 13:07_

If no one objects I can add it, not too hard.

---

_Comment by @chrisrodrigue on 2024-08-05 15:05_

There are a couple of options that can accomplish similar things (`--offline`, `--no-index`, and now  `--no-sources`).

Would `--offline` imply both `--no-index` and `--no-sources`? Or would it still allow local indices/sources? 


---

_Comment by @zanieb on 2024-08-05 17:38_

I think `--offline` and `--no-index` have no effect on local sources i.e. paths — these things have relatively distinct meanings even if in some cases they could have the same effect.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-05 18:25_

---

_Label `needs-decision` removed by @charliermarsh on 2024-08-05 18:25_

---

_Referenced in [astral-sh/uv#5801](../../astral-sh/uv/pulls/5801.md) on 2024-08-05 20:31_

---

_Closed by @charliermarsh on 2024-08-06 14:14_

---

---
number: 6839
title: "`banned-from` docs is confusing"
type: issue
state: closed
author: duongdominhchau
labels:
  - documentation
assignees: []
created_at: 2023-08-24T05:24:49Z
updated_at: 2023-08-29T00:51:16Z
url: https://github.com/astral-sh/ruff/issues/6839
synced_at: 2026-01-10T01:22:46Z
---

# `banned-from` docs is confusing

---

_Issue opened by @duongdominhchau on 2023-08-24 05:24_

> `banned-from`
>
> A list of modules that are allowed to be imported from

I read `banned-from = ["A"]` as "ban `from A import ...` statements". However, that doesn't match the description here.

> A list of modules that are allowed to be imported from

Is there a missing "not" here or am I misunderstanding something?

Link to relevant option: https://beta.ruff.rs/docs/settings/#flake8-import-conventions-banned-from

---

_Comment by @charliermarsh on 2023-08-24 14:04_

Yeah I think you're right, thank you -- I'll revise...

---

_Label `documentation` added by @charliermarsh on 2023-08-24 14:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-24 14:04_

---

_Referenced in [astral-sh/ruff#6959](../../astral-sh/ruff/pulls/6959.md) on 2023-08-29 00:42_

---

_Closed by @charliermarsh on 2023-08-29 00:51_

---

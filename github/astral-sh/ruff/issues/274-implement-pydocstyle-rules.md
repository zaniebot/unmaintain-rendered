---
number: 274
title: Implement pydocstyle rules
type: issue
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2022-09-28T21:31:42Z
updated_at: 2025-12-13T00:50:36Z
url: https://github.com/astral-sh/ruff/issues/274
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement pydocstyle rules

---

_Issue opened by @charliermarsh on 2022-09-28 21:31_

It's possible that we could collect the docstrings on the Rust side, then call out to pydocstyle. Or we could just re-implement the logic ourselves in Rust. The goal would be to enable adopting ruff for users of `flake8-docstring`.

See: https://github.com/PyCQA/pydocstyle


---

_Label `enhancement` added by @charliermarsh on 2022-09-28 21:31_

---

_Label `enhancement` removed by @charliermarsh on 2022-09-28 21:38_

---

_Label `rule` added by @charliermarsh on 2022-09-28 21:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-10 14:28_

---

_Comment by @charliermarsh on 2022-10-10 19:16_

Starting: #394, #395.

---

_Referenced in [astral-sh/ruff#396](../../astral-sh/ruff/pulls/396.md) on 2022-10-10 19:34_

---

_Referenced in [astral-sh/ruff#397](../../astral-sh/ruff/pulls/397.md) on 2022-10-10 20:04_

---

_Referenced in [astral-sh/ruff#404](../../astral-sh/ruff/pulls/404.md) on 2022-10-12 01:08_

---

_Referenced in [astral-sh/ruff#407](../../astral-sh/ruff/pulls/407.md) on 2022-10-12 15:06_

---

_Referenced in [astral-sh/ruff#408](../../astral-sh/ruff/pulls/408.md) on 2022-10-12 16:44_

---

_Referenced in [astral-sh/ruff#409](../../astral-sh/ruff/pulls/409.md) on 2022-10-12 17:20_

---

_Referenced in [astral-sh/ruff#411](../../astral-sh/ruff/pulls/411.md) on 2022-10-12 20:30_

---

_Referenced in [astral-sh/ruff#413](../../astral-sh/ruff/pulls/413.md) on 2022-10-12 21:12_

---

_Referenced in [astral-sh/ruff#425](../../astral-sh/ruff/pulls/425.md) on 2022-10-14 14:17_

---

_Referenced in [astral-sh/ruff#427](../../astral-sh/ruff/pulls/427.md) on 2022-10-14 15:52_

---

_Referenced in [astral-sh/ruff#429](../../astral-sh/ruff/pulls/429.md) on 2022-10-14 17:12_

---

_Closed by @charliermarsh on 2022-10-14 17:26_

---

_Comment by @matteosantama on 2022-10-15 03:56_

Wow you blitzed through these! The [rules](https://github.com/charliermarsh/ruff#rules) section of the `README.md` should be updated to reflect the latest coverage

---

_Comment by @charliermarsh on 2022-10-15 15:10_

Ah yeah, they're enumerated in that huge table (look for the `DXXX` codes), but the whole thing needs to be redone. Maybe for now I'll split that table into sections to make the rule sets clearer?


---

_Comment by @matteosantama on 2022-10-15 15:16_

Oh, no that's my bad! I thought the check mark meant "implemented" and the `DXXX` codes needed a check.

---

_Comment by @matteosantama on 2022-10-15 15:17_

As a follow-up though, is the best way to activate `pydocstyle` to manually enable those error codes? Or is there an "on-switch" for the plugin as a whole?

---

_Comment by @charliermarsh on 2022-10-15 15:18_

Oh that's great feedback... If the check mark confused you, then it's my bad, not yours :) Will think on that.

---

_Comment by @charliermarsh on 2022-10-15 15:18_

Yeah, the best way to activate (for now) is to manually enable the error code. I'm going to improve this.

Here's an example from Polars that I was (coincidentally) just writing up: https://github.com/pola-rs/polars/pull/5151#issuecomment-1279765121


---

_Referenced in [astral-sh/ruff#18073](../../astral-sh/ruff/pulls/18073.md) on 2025-05-13 17:18_

---

_Reopened by @dhruvmanila on 2025-05-14 19:55_

---

_Closed by @ntBre on 2025-12-13 00:50_

---

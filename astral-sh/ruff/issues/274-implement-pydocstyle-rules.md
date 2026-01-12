```yaml
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
synced_at: 2026-01-12T15:54:40Z
```

# Implement pydocstyle rules

---

_@charliermarsh_

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

_Reopened by @dhruvmanila on 2025-05-14 19:55_

---

_Closed by @ntBre on 2025-12-13 00:50_

---

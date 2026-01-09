---
number: 4429
title: Ignore E501 on comments (for black compatibility)
type: issue
state: closed
author: ricardoV94
labels: []
assignees: []
created_at: 2023-05-14T11:22:20Z
updated_at: 2023-05-14T11:33:03Z
url: https://github.com/astral-sh/ruff/issues/4429
synced_at: 2026-01-07T13:12:14-06:00
---

# Ignore E501 on comments (for black compatibility)

---

_Issue opened by @ricardoV94 on 2023-05-14 11:22_

I don't see a way to setup ruff so that it works like black wrt to line-length.

A comment [here](https://github.com/charliermarsh/ruff/issues/389#issuecomment-1528922079) suggested setting `E501` + `line-length=97`, which will work like black +10% rule.

However black does not fail when comments are too long. Is there a way to achieve the same with ruff so that they can be used interchangeably?



---

_Comment by @ricardoV94 on 2023-05-14 11:32_

Oh I found it explained here: https://beta.ruff.rs/docs/faq/#is-ruff-compatible-with-black

I guess this is not something that ruff wants to address.

---

_Closed by @ricardoV94 on 2023-05-14 11:33_

---

_Referenced in [astral-sh/ruff#5899](../../astral-sh/ruff/issues/5899.md) on 2023-07-19 22:54_

---

_Referenced in [rapidsai/cudf#15312](../../rapidsai/cudf/pulls/15312.md) on 2024-03-14 23:48_

---

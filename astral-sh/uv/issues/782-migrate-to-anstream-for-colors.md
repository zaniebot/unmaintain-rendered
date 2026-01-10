---
number: 782
title: Migrate to anstream for colors
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-01-04T15:43:37Z
updated_at: 2024-01-07T01:44:06Z
url: https://github.com/astral-sh/uv/issues/782
synced_at: 2026-01-10T01:23:04Z
---

# Migrate to anstream for colors

---

_Issue opened by @konstin on 2024-01-04 15:43_

Followup to #742.

---

_Comment by @charliermarsh on 2024-01-04 15:55_

I think anstream itself doesn't do colors, it does the println / writeable stream, and it integrates with styling crates like `owo-colors`.

---

_Comment by @BurntSushi on 2024-01-04 15:59_

I would check to see what Cargo is using for colors these days.

---

_Comment by @konstin on 2024-01-04 16:02_

anstream and color-print https://github.com/rust-lang/cargo/pull/12751

---

_Comment by @BurntSushi on 2024-01-04 16:02_

Looks like `anstream` and [`color-print`](https://docs.rs/color-print)?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-06 23:31_

---

_Referenced in [astral-sh/uv#823](../../astral-sh/uv/pulls/823.md) on 2024-01-07 01:27_

---

_Label `internal` added by @charliermarsh on 2024-01-07 01:29_

---

_Closed by @charliermarsh on 2024-01-07 01:44_

---

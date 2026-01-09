---
number: 17909
title: "Exclude `rust-toolchain.toml` from `sdist`"
type: issue
state: closed
author: MichaReiser
labels:
  - release
assignees: []
created_at: 2025-05-07T10:00:12Z
updated_at: 2025-06-17T13:58:17Z
url: https://github.com/astral-sh/ruff/issues/17909
synced_at: 2026-01-07T13:12:16-06:00
---

# Exclude `rust-toolchain.toml` from `sdist`

---

_Issue opened by @MichaReiser on 2025-05-07 10:00_

> That said, if you say that rust-toolchain.toml is for development and to consume optimizations early for your binary artifacts, would you consider excluding the file from the Python sdist since based on the premise (and based on your backwards compatibility claim) it's irrelevant to have it there for released packages.

Discussed in https://github.com/astral-sh/ruff/pull/17171#issuecomment-2857863325

---

_Comment by @MichaReiser on 2025-05-07 10:36_

The `rust-toolchain` was explicitly included in https://github.com/astral-sh/ruff/pull/8414

---

_Label `release` added by @MichaReiser on 2025-05-07 10:37_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-07 10:37_

---

_Referenced in [astral-sh/ruff#17171](../../astral-sh/ruff/pulls/17171.md) on 2025-05-07 10:37_

---

_Label `needs-decision` removed by @MichaReiser on 2025-05-07 16:57_

---

_Referenced in [astral-sh/ruff#17925](../../astral-sh/ruff/pulls/17925.md) on 2025-05-07 16:58_

---

_Closed by @ntBre on 2025-06-17 13:58_

---

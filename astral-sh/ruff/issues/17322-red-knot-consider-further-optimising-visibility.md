---
number: 17322
title: "[red-knot] Consider further optimising visibility constraints applied to `*`-import definitions"
type: issue
state: closed
author: AlexWaygood
labels:
  - performance
  - help wanted
  - ty
assignees: []
created_at: 2025-04-09T17:56:43Z
updated_at: 2025-04-15T11:32:23Z
url: https://github.com/astral-sh/ruff/issues/17322
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] Consider further optimising visibility constraints applied to `*`-import definitions

---

_Issue opened by @AlexWaygood on 2025-04-09 17:56_

https://github.com/astral-sh/ruff/pull/17317 reclaimed much of the performance regression from https://github.com/astral-sh/ruff/pull/17286, but it appears as though we have still lost a fair bit of performance over the last two days, much of it due to work on `*` imports. Here's an annotated graph of how the Codspeed [`red_knot_check_file[cold]`](https://codspeed.io/astral-sh/ruff/benchmarks/crates/ruff_benchmark/benches/red_knot.rs::check_file::benchmark_cold::red_knot_check_file%5Bcold%5D) benchmark has changed over the last two days:

![Image](https://github.com/user-attachments/assets/cb286bec-ba49-464c-9d22-7742cbd6d38a)

We may still be doing more work than necessary when it comes to applying and resolving visibility constraints for `*` imports. There are some suggestions in this thread for further optimisations that we could make: https://github.com/astral-sh/ruff/pull/17317#discussion_r2035680272

---

_Label `help wanted` added by @AlexWaygood on 2025-04-09 17:56_

---

_Label `performance` added by @AlexWaygood on 2025-04-09 17:56_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-09 17:56_

---

_Referenced in [astral-sh/ruff#17375](../../astral-sh/ruff/pulls/17375.md) on 2025-04-13 17:49_

---

_Closed by @AlexWaygood on 2025-04-15 11:32_

---

---
number: 7473
title: "tests: find a reliable strategy for testing system and managed interpreters"
type: issue
state: open
author: lucab
labels:
  - testing
assignees: []
created_at: 2024-09-17T17:16:41Z
updated_at: 2024-09-17T17:16:41Z
url: https://github.com/astral-sh/uv/issues/7473
synced_at: 2026-01-07T13:12:17-06:00
---

# tests: find a reliable strategy for testing system and managed interpreters

---

_Issue opened by @lucab on 2024-09-17 17:16_

Followup from https://github.com/astral-sh/uv/pull/7451.

As noted as a comment there in https://github.com/astral-sh/uv/pull/7451#discussion_r1763543095, some tests may require switching between managed and system interpreters.
While this works fairly well in CI, there is some implicit nondeterminism related to system setup and network fetches. This makes such tests brittle and unfriendly for other developers and packagers.

---

_Label `testing` added by @lucab on 2024-09-17 17:16_

---

_Referenced in [astral-sh/uv#7451](../../astral-sh/uv/pulls/7451.md) on 2024-09-17 17:34_

---

_Referenced in [astral-sh/uv#7596](../../astral-sh/uv/pulls/7596.md) on 2024-09-20 17:59_

---

_Referenced in [astral-sh/uv#12246](../../astral-sh/uv/pulls/12246.md) on 2025-03-17 19:53_

---

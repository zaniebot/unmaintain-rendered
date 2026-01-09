---
number: 7799
title: "Code generator doesn't preserve individual string quote style"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-10-04T04:54:40Z
updated_at: 2025-02-04T13:41:08Z
url: https://github.com/astral-sh/ruff/issues/7799
synced_at: 2026-01-07T13:12:15-06:00
---

# Code generator doesn't preserve individual string quote style

---

_Issue opened by @dhruvmanila on 2023-10-04 04:54_

This is related to the `Generator` struct.

This is especially true for triple-quoted strings. The `Generator` will normalize them to single quotes and perform some transformations which may or may not be correct:
* Escape the inner quotes if they match the outer ones
* Preserve the whitespaces (for example, using `\n`)

Related issues:
* https://github.com/astral-sh/ruff/issues/6988
* https://github.com/astral-sh/ruff/issues/7736
* https://github.com/astral-sh/ruff/issues/7720

---

_Label `bug` added by @dhruvmanila on 2023-10-04 04:54_

---

_Label `autofix` added by @dhruvmanila on 2023-10-04 04:54_

---

_Referenced in [astral-sh/ruff#9663](../../astral-sh/ruff/issues/9663.md) on 2024-01-28 15:36_

---

_Referenced in [astral-sh/ruff#9660](../../astral-sh/ruff/issues/9660.md) on 2024-01-28 17:05_

---

_Comment by @sanxiyn on 2024-02-15 13:37_

I am interested in working on this.

---

_Referenced in [astral-sh/ruff#10000](../../astral-sh/ruff/pulls/10000.md) on 2024-02-15 14:38_

---

_Referenced in [astral-sh/ruff#10256](../../astral-sh/ruff/pulls/10256.md) on 2024-03-06 19:39_

---

_Referenced in [astral-sh/ruff#10298](../../astral-sh/ruff/pulls/10298.md) on 2024-03-08 13:28_

---

_Referenced in [astral-sh/ruff#10314](../../astral-sh/ruff/pulls/10314.md) on 2024-03-10 10:12_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-05-06 12:16_

---

_Assigned to @ntBre by @ntBre on 2025-01-24 15:51_

---

_Unassigned @AlexWaygood by @ntBre on 2025-01-24 15:51_

---

_Referenced in [astral-sh/ruff#15726](../../astral-sh/ruff/pulls/15726.md) on 2025-01-24 16:55_

---

_Referenced in [astral-sh/ruff#15818](../../astral-sh/ruff/pulls/15818.md) on 2025-02-03 17:32_

---

_Closed by @ntBre on 2025-02-04 13:41_

---

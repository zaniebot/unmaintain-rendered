---
number: 18395
title: "Revisit character count for expressions in f-strings in `PYI053`"
type: issue
state: open
author: dylwil3
labels:
  - rule
assignees: []
created_at: 2025-05-30T19:40:47Z
updated_at: 2025-05-30T19:40:52Z
url: https://github.com/astral-sh/ruff/issues/18395
synced_at: 2026-01-07T13:12:16-06:00
---

# Revisit character count for expressions in f-strings in `PYI053`

---

_Issue opened by @dylwil3 on 2025-05-30 19:40_

We appear to count literal bytes in the source code snippet for the expression. It should probably at least be `chars`.

https://github.com/astral-sh/ruff/blob/ad024f9a09484beb17e194ea3ac877a45e6efaa7/crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs#L95

---

_Label `rule` added by @dylwil3 on 2025-05-30 19:40_

---

_Referenced in [astral-sh/ruff#17851](../../astral-sh/ruff/pulls/17851.md) on 2025-05-30 19:41_

---

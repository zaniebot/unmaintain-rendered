```yaml
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
synced_at: 2026-01-12T15:54:56Z
```

# Revisit character count for expressions in f-strings in `PYI053`

---

_@dylwil3_

We appear to count literal bytes in the source code snippet for the expression. It should probably at least be `chars`.

https://github.com/astral-sh/ruff/blob/ad024f9a09484beb17e194ea3ac877a45e6efaa7/crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs#L95

---

_Label `rule` added by @dylwil3 on 2025-05-30 19:40_

---

```yaml
number: 20849
title: Preserve unterminated string tokens
type: issue
state: closed
author: MichaReiser
labels:
  - parser
assignees: []
created_at: 2025-10-13T17:54:53Z
updated_at: 2025-10-15T07:50:58Z
url: https://github.com/astral-sh/ruff/issues/20849
synced_at: 2026-01-10T11:09:59Z
```

# Preserve unterminated string tokens

---

_Issue opened by @MichaReiser on 2025-10-13 17:54_

Unterminated strings are never emitted by the lexer which means that the parser can't recover from it. Possible solution would be move the handling of unterminated strings to the parser which could then emit a string node with Unterminated flag set on it. This is what Pyright does as well. 

---

_Renamed from "7. Unterminated strings are never emitted by the lexer which means that the parser can't recover from it. Possible solution would be move the handling of unterminated strings to the parser which could then emit a string node with Unterminated flag set on it. This is what Pyright does as well." to "Preserve unterminated string tokens" by @MichaReiser on 2025-10-13 17:55_

---

_Label `parser` added by @MichaReiser on 2025-10-13 17:55_

---

_Closed by @MichaReiser on 2025-10-15 07:50_

---

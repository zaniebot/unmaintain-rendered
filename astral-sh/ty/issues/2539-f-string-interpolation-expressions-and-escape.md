```yaml
number: 2539
title: F-string interpolation expressions and escape sequences not highlighted in strings
type: issue
state: open
author: chry-santhemum
labels:
  - server
assignees: []
created_at: 2026-01-16T23:12:33Z
updated_at: 2026-01-16T23:28:40Z
url: https://github.com/astral-sh/ty/issues/2539
synced_at: 2026-01-17T00:07:53Z
```

# F-string interpolation expressions and escape sequences not highlighted in strings

---

_@chry-santhemum_

In strings, the following are not highlighted:

1. Escape sequences like `\n`, `\t`, `\\` are shown in the same color as regular string text
2. In f-strings like f"""{keyname}""", the `{keyname}` expression is in the same color as regular string text

Expected: Escape sequences should have distinct coloring, and identifiers inside f-string braces should receive semantic token classification.

---

_Label `server` added by @AlexWaygood on 2026-01-16 23:28_

---

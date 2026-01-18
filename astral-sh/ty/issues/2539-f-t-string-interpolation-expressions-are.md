```yaml
number: 2539
title: F/T-string interpolation expressions are higlighted as string parts
type: issue
state: open
author: chry-santhemum
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-16T23:12:33Z
updated_at: 2026-01-18T11:07:00Z
url: https://github.com/astral-sh/ty/issues/2539
synced_at: 2026-01-18T11:18:08Z
```

# F/T-string interpolation expressions are higlighted as string parts

---

_@chry-santhemum_

In strings, the following are not highlighted:

1. ~~Escape sequences like `\n`, `\t`, `\\` are shown in the same color as regular string text~~ https://github.com/astral-sh/ty/issues/2552
2. In f-strings like f"""{keyname}""", the `{keyname}` expression is in the same color as regular string text

Expected: Escape sequences should have distinct coloring, and identifiers inside f-string braces should receive semantic token classification.

---

_Label `server` added by @AlexWaygood on 2026-01-16 23:28_

---

_Comment by @MichaReiser on 2026-01-18 11:03_

Thanks. These are two separate issues. One that escapes aren't highlighted separately and a second that expression parts in f and t-strings aren't highlighted differently

---

_Renamed from "F-string interpolation expressions and escape sequences not highlighted in strings" to "F/T-string interpolation expressions are higlighted as string parts" by @MichaReiser on 2026-01-18 11:05_

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-18 11:06_

---

_Label `bug` added by @MichaReiser on 2026-01-18 11:06_

---

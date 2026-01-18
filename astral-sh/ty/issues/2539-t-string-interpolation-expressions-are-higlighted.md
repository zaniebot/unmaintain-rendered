```yaml
number: 2539
title: T-string interpolation expressions are higlighted as string parts
type: issue
state: closed
author: chry-santhemum
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-16T23:12:33Z
updated_at: 2026-01-18T17:25:50Z
url: https://github.com/astral-sh/ty/issues/2539
synced_at: 2026-01-18T18:15:23Z
```

# T-string interpolation expressions are higlighted as string parts

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

_Renamed from "F/T-string interpolation expressions are higlighted as string parts" to "T-string interpolation expressions are higlighted as string parts" by @MichaReiser on 2026-01-18 11:16_

---

_Comment by @MichaReiser on 2026-01-18 11:17_

I'm unable to reproduce the f-string highlighting. ty correctly highlights the name expression. What editor are you using?

---

_Comment by @chry-santhemum on 2026-01-18 17:24_

Sorry, I meant to say T-strings in my original ask. Thanks!

---

_Closed by @AlexWaygood on 2026-01-18 17:25_

---

_Comment by @AlexWaygood on 2026-01-18 17:25_

(Fixed in https://github.com/astral-sh/ruff/pull/22674)

---

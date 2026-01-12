```yaml
number: 1000
title: Smart formatting of types
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-08-15T12:40:25Z
updated_at: 2025-08-19T17:31:45Z
url: https://github.com/astral-sh/ty/issues/1000
synced_at: 2026-01-12T15:54:24Z
```

# Smart formatting of types

---

_@MichaReiser_

Smart formatting of complex types for improved readability (esp. signatures of callables) with a focus on the LSP, but it would also be nice if some of it could be ported to CLI diagnostics.

We don't have to but we could use our formatter infrastructure for this.

Example of a not so readable type in https://github.com/astral-sh/ty/issues/264#issuecomment-3185997723

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:40_

---

_Label `server` added by @MichaReiser on 2025-08-15 12:40_

---

_Assigned to @Gankra by @Gankra on 2025-08-15 13:25_

---

_Comment by @Gankra on 2025-08-15 13:31_

I did a rough draft of this locally for functions but concluded we probably want to tweak the type/signature display machinery to take formatting settings -- notably whether line-breaking would be appropriate or not, as many places aren't expecting it.

---

_Closed by @Gankra on 2025-08-19 17:31_

---

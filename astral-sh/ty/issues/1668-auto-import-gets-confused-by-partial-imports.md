```yaml
number: 1668
title: auto-import gets confused by partial imports
type: issue
state: closed
author: Gankra
labels:
  - bug
  - server
  - imports
assignees: []
created_at: 2025-11-28T16:55:57Z
updated_at: 2025-12-01T19:20:49Z
url: https://github.com/astral-sh/ty/issues/1668
synced_at: 2026-01-12T15:54:25Z
```

# auto-import gets confused by partial imports

---

_@Gankra_

### Summary

Having `import warnings` in the file makes it unable to suggest `from warnings import deprecated` anymore:

<img width="487" height="181" alt="Image" src="https://github.com/user-attachments/assets/bbed1c98-b0e2-45b2-8e74-1649dc17b5e5" />

<img width="479" height="169" alt="Image" src="https://github.com/user-attachments/assets/b189c035-654a-4d49-882a-d1e2077d498e" />

https://play.ty.dev/bbf922d5-0526-4ada-b89f-c66e56a06579

---

_Label `bug` added by @Gankra on 2025-11-28 16:55_

---

_Label `server` added by @Gankra on 2025-11-28 16:55_

---

_Label `imports` added by @Gankra on 2025-11-28 16:55_

---

_Comment by @BurntSushi on 2025-12-01 13:10_

Related: https://github.com/astral-sh/ty/issues/1274#issuecomment-3472864360

Completions at least do still suggest `deprecated` from `warnings`, but 1) it isn't the first result and 2) if you select it, it uses the fully qualified name:

https://github.com/user-attachments/assets/030aab78-a74d-4e43-8f9b-7214f3f72915

I'm not sure why this causes the quick fix to drop the suggestion though.

---

_Comment by @Gankra on 2025-12-01 14:15_

Oh interesting, sounds like my implementation of this functionality is holding auto-complete wrong (or just running afoul of assumptions that make sense for auto-complete's more holistic approach).

I would certainly love if we could also suggest "qualify as..." as a quickfix too.

---

_Closed by @BurntSushi on 2025-12-01 19:20_

---

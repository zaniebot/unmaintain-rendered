```yaml
number: 1964
title: Is it possible to shorten/truncate inlay type hints for unions (in Zed)?
type: issue
state: open
author: toddpocuca
labels:
  - question
assignees: []
created_at: 2025-12-16T23:01:32Z
updated_at: 2025-12-16T23:33:26Z
url: https://github.com/astral-sh/ty/issues/1964
synced_at: 2026-01-12T15:54:26Z
```

# Is it possible to shorten/truncate inlay type hints for unions (in Zed)?

---

_@toddpocuca_

### Question

I work with JAX and often libraries in the ecosystem will accept/return many objects because of types like `ArrayTree` or `ArrayLike`, which are long unions and produce type hints like:

<img width="959" height="97" alt="Image" src="https://github.com/user-attachments/assets/abc2e13e-cae4-4b5a-890b-74773330b457" />

Is there any way to shorten the number of union elements included in the inlay hints?

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `question` added by @toddpocuca on 2025-12-16 23:01_

---

_Comment by @Gankra on 2025-12-16 23:28_

Hmm yeah that's a bit much in this case, we should consider more aggressive heuristics for truncating these things in multi-assigns.

---

_Comment by @AlexWaygood on 2025-12-16 23:33_

Treating PEP-613 aliases as first-class opaque types and just referring to them with the alias names (rather than eagerly expanding the values of the aliases) would probably help a lot here. Similar to #1883

---

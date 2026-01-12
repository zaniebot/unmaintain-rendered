```yaml
number: 1510
title: Type inference for generator expressions
type: issue
state: closed
author: AlexWaygood
labels: []
assignees: []
created_at: 2025-11-10T10:12:58Z
updated_at: 2025-11-14T13:04:13Z
url: https://github.com/astral-sh/ty/issues/1510
synced_at: 2026-01-12T15:54:25Z
```

# Type inference for generator expressions

---

_@AlexWaygood_

I think these should be basically the same as comprehensions (https://github.com/astral-sh/ruff/pull/20962, https://github.com/astral-sh/ty/issues/1388), except that `types.GeneratorType` is covariant. That means that Literal promotion probably won't be necessary, bidirectional inference is less important, and unioning with `Unknown` probably won't be necessary 

---

_Added to milestone `Beta` by @AlexWaygood on 2025-11-10 10:12_

---

_Assigned to @sharkdp by @MichaReiser on 2025-11-10 16:25_

---

_Closed by @sharkdp on 2025-11-14 13:04_

---

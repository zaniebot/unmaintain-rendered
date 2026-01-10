```yaml
number: 527
title: Consider a strict mode for ty
type: issue
state: open
author: nth10sd
labels:
  - configuration
  - typing semantics
assignees: []
created_at: 2025-05-27T23:53:22Z
updated_at: 2025-12-31T16:47:20Z
url: https://github.com/astral-sh/ty/issues/527
synced_at: 2026-01-10T01:56:40Z
```

# Consider a strict mode for ty

---

_Issue opened by @nth10sd on 2025-05-27 23:53_

### Question

Will ty consider having a strict mode, similar to the one found in `mypy`?

Asking because `division-by-zero` was [disabled by default](https://github.com/astral-sh/ruff/pull/18220), and parsing the rules reference [to turn on everything](https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md) seems less than optimal.

`pyright` / `basedpyright` also has different levels of modes.

### Version

0.0.1-alpha.7

---

_Label `question` added by @nth10sd on 2025-05-27 23:53_

---

_Comment by @MichaReiser on 2025-05-28 06:13_

> rules reference [to turn on everything](https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md?rgh-link-date=2025-05-27T23%3A53%3A22.000Z) seems less than optimal.

We do have plans to simplify opting in to entire categories (e.g. stricter rules). It's probably going to be different from a single `strict = true` option.

---

_Label `configuration` added by @AlexWaygood on 2025-05-28 06:50_

---

_Comment by @DetachHead on 2025-06-11 22:17_

related: #174

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 09:14_

---

_Label `question` removed by @MichaReiser on 2025-11-14 09:14_

---

_Label `typing semantics` added by @MichaReiser on 2025-11-14 09:14_

---

_Comment by @MichaReiser on 2025-12-06 16:25_

From a setting perspective. I assume that a strict mode setting would be global for the entire project and can't be overriden (because it can change the inferred types of imported modules)

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:47_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:47_

---

_Removed from milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:47_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 16:47_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:47_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:47_

---

```yaml
number: 2082
title: config setting to resolve some modules to Any
type: issue
state: open
author: carljm
labels:
  - configuration
  - imports
assignees: []
created_at: 2025-12-18T18:01:41Z
updated_at: 2026-01-09T09:47:27Z
url: https://github.com/astral-sh/ty/issues/2082
synced_at: 2026-01-12T15:54:26Z
```

# config setting to resolve some modules to Any

---

_@carljm_

Similar to https://pyrefly.org/en/docs/configuration/#replace-imports-with-any%20option

This is useful for highly-dynamic and/or badly-typed third-party modules that you just want to not get false positives from, ever.

Would be a solution to #2076 

Related but not the same as https://github.com/astral-sh/ty/issues/1354

---

_Label `configuration` added by @carljm on 2025-12-18 18:01_

---

_Label `imports` added by @carljm on 2025-12-18 18:01_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-18 18:03_

---

_Comment by @MichaReiser on 2025-12-19 12:43_

One wrinkle here is that it would still be nice to keep the types for completions, find references and such. But that's obviously rather tricky

---

_Comment by @AlexWaygood on 2025-12-19 12:47_

> One wrinkle here is that it would still be nice to keep the types for completions, find references and such. But that's obviously rather tricky

Not necessarily _that_ tricky... we could add another `DynamicType` variant that carries around our "best-guess" type inside it (and uses that for completions), but acts like `Any` in that it always results in zero diagnostics when type-checking?

---

_Comment by @MichaReiser on 2026-01-09 09:47_

I'll bump the priority on this because it has come up a few times and I suspect it's very easy to implement.

---

_Removed from milestone `Stable` by @MichaReiser on 2026-01-09 09:47_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-09 09:47_

---

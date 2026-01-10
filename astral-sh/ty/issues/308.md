```yaml
number: 308
title: "Suggest type annotations or \"better\" types for existing annotations"
type: issue
state: open
author: jack-mcivor
labels:
  - wish
assignees: []
created_at: 2025-05-10T14:56:12Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/308
synced_at: 2026-01-10T01:58:59Z
```

# Suggest type annotations or "better" types for existing annotations

---

_Issue opened by @jack-mcivor on 2025-05-10 14:56_

Could ty analyze variable usage to suggest type annotations or "better" types for existing annotations?

I have a couple of motivating cases in mind where annotations could be improved on function arguments:
* Broadening collection types: for arguments typed as `list[T]`, if usage only requires common sequence/iterable operations, suggest `Sequence[T]` or `Iterable[T]`. This allows broader input types (e.g., tuples, generators).
* Inferring literal types: for arguments typed as `str`, if the function body checks against a fixed set of string values, suggest a `Literal` type (eg. `Literal["bisect", "newton", "brentq"]`). This clarifies valid inputs for callers.

AFAIK existing type checking tools cannot do this - it goes beyond checking type correctness and so is probably outside the scope of most tools I know of.

---

_Label `question` added by @jack-mcivor on 2025-05-10 14:56_

---

_Label `wish` added by @MichaReiser on 2025-05-12 06:47_

---

_Label `question` removed by @MichaReiser on 2025-05-12 06:47_

---

_Comment by @T-256 on 2025-05-13 02:58_

That will be a linter with autofix functionality which based on and uses type-checker capabilities.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

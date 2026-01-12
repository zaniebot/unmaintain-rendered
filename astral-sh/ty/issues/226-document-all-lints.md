```yaml
number: 226
title: Document all lints
type: issue
state: closed
author: MichaReiser
labels:
  - documentation
assignees: []
created_at: 2024-12-10T11:16:12Z
updated_at: 2025-05-22T15:26:54Z
url: https://github.com/astral-sh/ty/issues/226
synced_at: 2026-01-12T15:54:22Z
```

# Document all lints

---

_@MichaReiser_

With https://github.com/astral-sh/ruff/pull/14873, it's now required to document every lint. The PR itself documented some lints but not all of them. 

The goal of this issue is to go through all lints and add documentation to them, similar to what we have in ruff.

### Lint rules

_The following table only includes the lint rules that have incomplete documentation._

| Rule                                | "What it does" | "Why is this bad?" | "Examples" |
| --------                            | --------       | --------           | --------   |
| `CALL_POSSIBLY_UNBOUND_METHOD`      | ✅             | ✅                  |            |
| `INVALID_BASE`                      |                |                    |            |


---

_Label `documentation` added by @MichaReiser on 2024-12-10 11:16_

---

_Comment by @InSyncWithFoo on 2024-12-11 21:29_

Is this `help-wanted`? There are currently 21 rules with `/// TODO #14889`; that should only take up one of my afternoons or so.

---

_Comment by @MichaReiser on 2024-12-12 07:02_

Thanks for asking. IMO, it has fairly low priority at the moment and we don't even know if we'll keep the rules as they are right now. That's why I probably would wait.

---

_Renamed from "[red-knot] Document all lints" to "Document all lints" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @InSyncWithFoo on 2025-05-09 17:22_

The table should be updated now. There's only a few left after [#17981](https://github.com/astral-sh/ruff/pull/17981).

As per the discussion there, there are a few lints that need refactoring. The most problematic would be `invalid-type-form`, which uses "type form" incorrectly (a "type form" is supposed to be a runtime object that encodes a type in the type system) and has numerous references.

---

_Comment by @dhruvmanila on 2025-05-13 20:57_

(Removed the rules from the table where the three sections are present.)

---

_Comment by @AlexWaygood on 2025-05-22 15:26_

I think this is now all done following https://github.com/astral-sh/ruff/pull/18017 and https://github.com/astral-sh/ruff/pull/18245. Thank you so much for your work on this @InSyncWithFoo!

---

_Closed by @AlexWaygood on 2025-05-22 15:26_

---

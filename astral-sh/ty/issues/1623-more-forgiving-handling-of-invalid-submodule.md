```yaml
number: 1623
title: more forgiving handling of invalid submodule-attribute access
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-11-24T17:36:11Z
updated_at: 2025-11-24T20:28:41Z
url: https://github.com/astral-sh/ty/issues/1623
synced_at: 2026-01-12T15:54:25Z
```

# more forgiving handling of invalid submodule-attribute access

---

_@carljm_

if we hit this branch, we have pretty high confidence that we know what the user _meant_ here -- we could even consider inferring the module-literal type of the submodule rather than inferring `Unknown`. (Or `<submodule type> | Unknown`...?)

And since this often will (by pure luck!) work at runtime, we could also consider downgrading the diagnostic to warning-level rather than error-level

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/21561#discussion_r2553149726_
            

---

_Added to milestone `Stable` by @carljm on 2025-11-24 17:36_

---

_Comment by @carljm on 2025-11-24 17:37_

I think it's a good idea to go ahead and infer the submodule type instead of `Unknown` (I don't think there even needs to be a union with `Unknown` or anything like that.) And I think it would make sense to downgrade the severity of the diagnostic, as well.

---

_Comment by @AlexWaygood on 2025-11-24 20:28_

> And since this often will (by pure luck!) work at runtime, we could also consider downgrading the diagnostic to warning-level rather than error-level

this bit was done in https://github.com/astral-sh/ruff/pull/21618

---

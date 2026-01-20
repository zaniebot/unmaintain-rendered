```yaml
number: 1922
title: Split error for empty function body into its own error code
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-16T12:30:48Z
updated_at: 2026-01-20T13:46:53Z
url: https://github.com/astral-sh/ty/issues/1922
synced_at: 2026-01-20T14:40:26Z
```

# Split error for empty function body into its own error code

---

_@AlexWaygood_

Pyright allows functions to unsoundly have empty bodies even if they have a non-`None` return type, and mypy used to allow this too. As a result, we've seen several instances of users being confused by the fact that we do not allow this. For example:

```py
def foo() -> int: ...  # error[invalid-return-type]
```

It's a deliberate decision from us to be more strict than pyright here, since this function obviously does not return an `int` at runtime. We already have a [subdiagnostic](https://github.com/astral-sh/ruff/blob/5b1d3ac9b9c458448e388075945869a4d49296a9/crates/ty_python_semantic/src/types/diagnostic.rs#L2676-L2679) message that explains the specific conditions in which we do allow functions to have empty bodies. However, splitting this into a separate `empty-body` error code would allow users who are heavily impacted by this inconsistency to switch it off on their codebase (even just temporarily while they work down the number of errors). This would also match mypy's behaviour (mypy has a dedicated `empty-body` error code.



---

_Label `diagnostics` added by @AlexWaygood on 2025-12-16 12:30_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 14:30_

---

_Comment by @maifeeulasad on 2026-01-20 08:47_

Can I work on this one?

---

_Comment by @AlexWaygood on 2026-01-20 08:48_

@maifeeulasad sure!

---

_Assigned to @maifeeulasad by @AlexWaygood on 2026-01-20 08:48_

---

_Comment by @maifeeulasad on 2026-01-20 13:38_

Dear @AlexWaygood 

I have created an issue upstream in `ruff`. Which is already linked here. Now waiting for their feedback. Either we can do it like this. Or I can just push my code update the submodule hash, which may become problamatic in future.

Any thought on this??

---

_Comment by @AlexWaygood on 2026-01-20 13:46_

hey @maifeeulasad -- sorry for the confusion! All the source code for ty is found in the Ruff respository, so to fix this issue you need to make a PR directly to the Ruff repository. Opening an issue over at Ruff isn't necessary, because that repository is maintained by the same team that works on ty. We update the submodule hash in this repository every time we make a release. You can see some more details on contributing to ty here: https://github.com/astral-sh/ruff/blob/main/crates/ty/CONTRIBUTING.md.

---

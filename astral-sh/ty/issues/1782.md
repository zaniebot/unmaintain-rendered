```yaml
number: 1782
title: "Remove `tests` from the `environment.root` default"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - imports
assignees: []
created_at: 2025-12-05T18:09:44Z
updated_at: 2025-12-12T16:59:08Z
url: https://github.com/astral-sh/ty/issues/1782
synced_at: 2026-01-10T01:55:00Z
```

# Remove `tests` from the `environment.root` default

---

_Issue opened by @MichaReiser on 2025-12-05 18:09_

ty currently adds `tests` as a search path if the user didn't specify any. This is problematic because it prevents ty from detecting test modules being imported into production code.

The main reason we added the fallback `tests` was to make imports work within the test folders. This is no longer required now that we have desperate module resolution. 

We should try changing the default and see how big the fallout on mypy primer is. If it's small, I suggest landing this change before the beta.

We should also update ty's [module resolution documentation](https://docs.astral.sh/ty/modules/#first-party-modules) to mention the new desperate module resolution algorithm.



---

_Comment by @MichaReiser on 2025-12-05 18:09_

@Gankra is this something you can look into next week?

---

_Label `configuration` added by @MichaReiser on 2025-12-05 18:09_

---

_Added to milestone `Beta` by @MichaReiser on 2025-12-05 18:10_

---

_Label `imports` added by @AlexWaygood on 2025-12-05 18:16_

---

_Assigned to @Gankra by @Gankra on 2025-12-05 18:35_

---

_Closed by @BurntSushi on 2025-12-12 16:59_

---

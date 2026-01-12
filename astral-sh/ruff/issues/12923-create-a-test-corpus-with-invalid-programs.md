```yaml
number: 12923
title: Create a test corpus with invalid programs
type: issue
state: closed
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
created_at: 2024-08-16T10:10:30Z
updated_at: 2025-01-15T09:50:17Z
url: https://github.com/astral-sh/ruff/issues/12923
synced_at: 2026-01-12T15:54:52Z
```

# Create a test corpus with invalid programs

---

_@MichaReiser_

Invalid programs are the default-state in editors. We should add extensive tests on how red knot handles invalid programs.

---

_Label `red-knot` added by @MichaReiser on 2024-08-16 10:10_

---

_Label `testing` added by @AlexWaygood on 2024-11-07 18:49_

---

_Comment by @MichaReiser on 2025-01-15 09:50_

I don't think we have very extensive tests right now but we do run red knot over all parser tests of which many cover invalid syntax. That seems good enough to me for now. We'll want to add more tests in the future that assert how red knot infers types for invalid programs but this isn't a goal for now.

---

_Closed by @MichaReiser on 2025-01-15 09:50_

---

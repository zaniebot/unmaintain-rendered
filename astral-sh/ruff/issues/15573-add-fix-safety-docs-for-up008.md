```yaml
number: 15573
title: Add fix safety docs for UP008
type: issue
state: closed
author: joecox
labels:
  - documentation
  - fixes
assignees: []
created_at: 2025-01-18T21:39:33Z
updated_at: 2025-04-21T13:48:06Z
url: https://github.com/astral-sh/ruff/issues/15573
synced_at: 2026-01-12T15:54:54Z
```

# Add fix safety docs for UP008

---

_@joecox_

Hello, as of Ruff 0.9.2, `UP008` produces unsafe fixes. Can the [documentation](https://docs.astral.sh/ruff/rules/super-call-with-parameters/) be updated to explain the fix safety, similar to rules like [`UP006`](https://docs.astral.sh/ruff/rules/non-pep585-annotation/#fix-safety)?

---

_Renamed from "Add fix safety to UP008" to "Add fix safety docs for UP008" by @joecox on 2025-01-18 21:39_

---

_Comment by @dylwil3 on 2025-01-18 22:29_

I don't see any recent changes to `UP008`. Can you give an example of what you mean?

---

_Label `needs-mre` added by @dylwil3 on 2025-01-18 22:34_

---

_Comment by @charliermarsh on 2025-01-18 23:03_

I think it's been unsafe for a while, though it'd be good to document why.

---

_Label `needs-mre` removed by @charliermarsh on 2025-01-18 23:03_

---

_Label `documentation` added by @charliermarsh on 2025-01-18 23:03_

---

_Label `fixes` added by @AlexWaygood on 2025-01-24 15:01_

---

_Comment by @VascoSch92 on 2025-04-20 21:14_

I think this issue is (partially) closed by the PR #17441 

---

_Closed by @ntBre on 2025-04-21 13:48_

---

```yaml
number: 457
title: Improve hint for unresolved-import diagnostic
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - diagnostics
  - imports
assignees: []
created_at: 2025-05-20T09:05:54Z
updated_at: 2025-08-26T15:01:18Z
url: https://github.com/astral-sh/ty/issues/457
synced_at: 2026-01-10T02:06:24Z
```

# Improve hint for unresolved-import diagnostic

---

_Issue opened by @MichaReiser on 2025-05-20 09:05_

https://github.com/astral-sh/ruff/pull/18207/ added a hint to the `unresolved-import` diagnostic, suggesting users to correctly configure their venv. While useful, I think we could do better by:


* Adding a note (or sub diagnostic?) explaining where ty searched for said import (which search paths are configured?)
  * first-party (if any): `src.root` 
  * third-party: `enviroment.python`: Ideally, with an information how we detected said environment and how they could customize it
  * extra search paths (if any)
  * standard library (typeshed path)
* Add a note if there's no `third-party` import search path, hinting users to how they can configure it

I'm not sure how the best rendering looks like. You'll have to do some exploration on how to query and present the information to the user, so that it's useful.

We should also replace the URL to the documentation and use a redirect (I can set this up)



---

_Label `help wanted` added by @MichaReiser on 2025-05-20 09:05_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-20 09:05_

---

_Label `imports` added by @MichaReiser on 2025-05-20 09:05_

---

_Closed by @BurntSushi on 2025-08-26 15:01_

---

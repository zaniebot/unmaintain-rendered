```yaml
number: 17576
title: "[red-knot] Use opt-level 3 for mypy primer runs"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ci
  - ty
assignees: []
draft: true
base: main
head: micha/mypy-primer-opt-level
created_at: 2025-04-23T07:51:33Z
updated_at: 2025-05-03T17:46:27Z
url: https://github.com/astral-sh/ruff/pull/17576
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Use opt-level 3 for mypy primer runs

---

_@MichaReiser_

_No description provided._

---

_Comment by @github-actions[bot] on 2025-04-23 07:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-04-23 08:08_

The benefits here seem very limited. I suspect that we're mainly running into an IO limit (crawling all files/cloning)... Or the env variables don't make it through to cargo for whatever reason

---

_Comment by @MichaReiser on 2025-04-23 08:38_

Okay, it does pick up the right opt level (I verified this with a failing build script that logs the OPT_LEVEL env variable). That means that a release build just doesn't give us back that much

---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 08:46_

---

_Label `ci` added by @AlexWaygood on 2025-04-23 08:46_

---

_Closed by @MichaReiser on 2025-04-23 09:01_

---

_Branch deleted on 2025-05-03 17:46_

---

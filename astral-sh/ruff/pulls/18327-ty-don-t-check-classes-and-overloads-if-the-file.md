```yaml
number: 18327
title: "[ty] Don't check classes and overloads if the file isn't first-party code"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: alex/unnecessary-class-checks
created_at: 2025-05-26T19:45:22Z
updated_at: 2025-06-28T14:13:46Z
url: https://github.com/astral-sh/ruff/pull/18327
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Don't check classes and overloads if the file isn't first-party code

---

_@AlexWaygood_

_No description provided._

---

_Label `performance` added by @AlexWaygood on 2025-05-26 19:45_

---

_Label `ty` added by @AlexWaygood on 2025-05-26 19:45_

---

_Comment by @github-actions[bot] on 2025-05-26 19:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-26 19:53_

I'm unsure if we want to do this before resolving https://github.com/astral-sh/ty/issues/114

---

_Comment by @AlexWaygood on 2025-05-26 20:00_

I'm not seeing any speedup with this branch using the `ty_benchmark` script even when running ty on codebases that have large 3rd-party dependencies (e.g. pandas). So I'm fine to leave this for now.

---

_Closed by @AlexWaygood on 2025-05-26 20:00_

---

_Branch deleted on 2025-05-26 20:01_

---

_Branch restored on 2025-06-28 13:42_

---

_Reopened by @AlexWaygood on 2025-06-28 13:42_

---

_Closed by @AlexWaygood on 2025-06-28 14:13_

---

_Branch deleted on 2025-06-28 14:13_

---

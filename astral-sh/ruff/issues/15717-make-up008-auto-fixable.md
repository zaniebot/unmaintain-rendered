---
number: 15717
title: Make UP008 auto-fixable
type: issue
state: closed
author: DaniBodor
labels:
  - fixes
assignees: []
created_at: 2025-01-24T14:37:38Z
updated_at: 2025-01-24T15:01:25Z
url: https://github.com/astral-sh/ruff/issues/15717
synced_at: 2026-01-10T01:22:56Z
---

# Make UP008 auto-fixable

---

_Issue opened by @DaniBodor on 2025-01-24 14:37_

### Description

To me it looks like [this](https://docs.astral.sh/ruff/rules/super-call-with-parameters/) should be safe to fix automatically.


---

_Label `fixes` added by @AlexWaygood on 2025-01-24 14:57_

---

_Comment by @AlexWaygood on 2025-01-24 14:58_

Hmm, the documentation for this rule states:

> Fix is always available.

---

_Comment by @MichaReiser on 2025-01-24 14:59_

There's always a fix but the fix is unsafe. You may have to opt in to applying the fix using `--unsafe-fixes`

---

_Comment by @AlexWaygood on 2025-01-24 15:01_

Closing as a duplicate of https://github.com/astral-sh/ruff/issues/15573 :-)

---

_Closed by @AlexWaygood on 2025-01-24 15:01_

---

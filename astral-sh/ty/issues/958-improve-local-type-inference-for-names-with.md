---
number: 958
title: improve local type inference for names with nonlocal writes in nested scopes
type: issue
state: closed
author: carljm
labels:
  - type-inference
assignees: []
created_at: 2025-08-08T21:20:07Z
updated_at: 2026-01-09T02:21:29Z
url: https://github.com/astral-sh/ty/issues/958
synced_at: 2026-01-10T01:48:23Z
---

# improve local type inference for names with nonlocal writes in nested scopes

---

_Issue opened by @carljm on 2025-08-08 21:20_

After https://github.com/astral-sh/ruff/pull/19821 we will include types from nested-scope nonlocal bindings in the public type of a symbol, but we still won't consider them in local reads of such symbols. See https://github.com/astral-sh/ruff/pull/19821#issuecomment-3169019532 and https://github.com/astral-sh/ruff/pull/19821#issuecomment-3169316459 for examples of code where we get type inference wrong due to this, and ideas on how we could improve it.

I don't think we should prioritize this until/unless we see it coming up in real-world bug reports. (For example, pyright doesn't do this, and gets these types wrong in the same way we do, which suggests it hasn't been a major issue?). Just wanted to capture the issue here for posterity.

---

_Label `type-inference` added by @carljm on 2025-08-08 21:20_

---

_Comment by @carljm on 2026-01-09 02:21_

Closing this; it's based on the assumption that a PR would land that we ended up not landing

---

_Closed by @carljm on 2026-01-09 02:21_

---

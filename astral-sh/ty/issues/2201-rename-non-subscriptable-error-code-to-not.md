---
number: 2201
title: "Rename `non-subscriptable` error code to `not-subscriptable`"
type: issue
state: closed
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-24T12:32:52Z
updated_at: 2025-12-27T11:44:36Z
url: https://github.com/astral-sh/ty/issues/2201
synced_at: 2026-01-10T01:51:14Z
---

# Rename `non-subscriptable` error code to `not-subscriptable`

---

_Issue opened by @AlexWaygood on 2025-12-24 12:32_

`non-subscriptable` is inconsistent with other error codes such as `not-iterable`, and I prefer `not-` to `non-` (it reads more like English).

We should get this in before our stable release because it's a breaking change for people who have this error code suppressed (either inline or in their config file)

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-24 12:32_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-24 12:32_

---

_Closed by @AlexWaygood on 2025-12-27 11:44_

---

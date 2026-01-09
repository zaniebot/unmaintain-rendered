---
number: 19263
title: Stabilize the new output formats
type: issue
state: open
author: ntBre
labels:
  - breaking
  - diagnostics
assignees: []
created_at: 2025-07-10T15:54:29Z
updated_at: 2025-10-02T12:39:32Z
url: https://github.com/astral-sh/ruff/issues/19263
synced_at: 2026-01-07T13:12:16-06:00
---

# Stabilize the new output formats

---

_Issue opened by @ntBre on 2025-07-10 15:54_

The new output formats built on the new diagnostic model are currently gated behind preview. We should stabilize them in the next minor release. See https://github.com/astral-sh/ruff/pull/19133 as one example.

---

_Added to milestone `v0.13` by @ntBre on 2025-07-10 15:54_

---

_Label `breaking` added by @ntBre on 2025-07-10 15:54_

---

_Label `diagnostics` added by @ntBre on 2025-07-10 15:54_

---

_Referenced in [astral-sh/ruff#19133](../../astral-sh/ruff/pulls/19133.md) on 2025-07-10 15:54_

---

_Comment by @ntBre on 2025-09-05 15:36_

This is still only used by the JSON output format, but it was added in the middle of the last release cycle. I think we can defer this until 0.14 both to give JSON users more time to report issues and to make sure we don't need the `preview` diagnostic configuration option for anything else. Currently, stabilizing the small JSON preview changes would leave `DisplayDiagnosticConfig::preview` completely unused.

---

_Removed from milestone `v0.13` by @ntBre on 2025-09-05 15:36_

---

_Added to milestone `v0.14` by @ntBre on 2025-09-05 15:36_

---

_Referenced in [InSyncWithFoo/ryecharm#130](../../InSyncWithFoo/ryecharm/issues/130.md) on 2025-09-06 04:09_

---

_Removed from milestone `v0.14` by @ntBre on 2025-10-02 12:39_

---

_Added to milestone `v0.15` by @ntBre on 2025-10-02 12:39_

---

---
number: 8887
title: "Formatter: `walrus_subscript` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-11-29T00:49:35Z
updated_at: 2023-12-04T05:34:43Z
url: https://github.com/astral-sh/ruff/issues/8887
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `walrus_subscript` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 00:49_

Implement Black's [`walrus_subscript`](https://github.com/psf/black/pull/3823) preview style.

Ruff already ships the fix in its stable style but there's a difference for parenthesized walrus operators. I created [an issue](https://github.com/psf/black/issues/4078) in the Black repository to align on the desired style. 

We should copy over black's new tests and fix the deviation if necessary

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 00:49_

---

_Renamed from "`walrus_subscript` Implemented, but may require a bugfix for parenthesized walrus operators. Ruff keeps the whitespace in front of the `:` (`x[(a := 0) :]`), black removes it (`x[(a := 0):]`)." to "Formatter: `walrus_subscript` preview style" by @MichaReiser on 2023-11-29 00:49_

---

_Label `formatter` added by @MichaReiser on 2023-11-29 00:52_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 00:52_

---

_Label `help wanted` added by @MichaReiser on 2023-11-29 00:52_

---

_Comment by @MichaReiser on 2023-12-04 05:34_

Acknowledge as a bug in black. Closing as complete

---

_Closed by @MichaReiser on 2023-12-04 05:34_

---

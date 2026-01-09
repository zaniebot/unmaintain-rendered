---
number: 18296
title: "Documentation A11y: iconography for rule status is difficult to read in dark mode"
type: issue
state: closed
author: MaddyGuthridge
labels:
  - documentation
assignees: []
created_at: 2025-05-25T12:45:37Z
updated_at: 2025-05-26T14:35:21Z
url: https://github.com/astral-sh/ruff/issues/18296
synced_at: 2026-01-07T13:12:16-06:00
---

# Documentation A11y: iconography for rule status is difficult to read in dark mode

---

_Issue opened by @MaddyGuthridge on 2025-05-25 12:45_

Currently, the iconography showing the status of a rule (stable, unstable) is a little challenging to read when dark mode. In particular the ✔️ emoji does not have very good contrast, and they greyed-out 🛠️ emoji is a little unclear imo.

A simple fix is:

* Swap the ✔️ for a ✅, or don't show anything at all (stable is assumed by most to be the default)
* Instead of showing a greyed-out 🛠️, don't show anything at all

I'm currently waiting for `rustc` to build `ruff` so I can try my hand at fixing this (update: my laptop is not happy with me, `rust-analyzer` generated 17 GB of temporary files while trying to compile it 💀)

---

_Referenced in [astral-sh/ruff#18297](../../astral-sh/ruff/pulls/18297.md) on 2025-05-25 13:14_

---

_Label `documentation` added by @AlexWaygood on 2025-05-25 13:19_

---

_Closed by @MichaReiser on 2025-05-26 14:35_

---

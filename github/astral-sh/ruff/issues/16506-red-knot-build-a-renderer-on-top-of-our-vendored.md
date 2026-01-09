---
number: 16506
title: "[red-knot] Build a renderer, on top of our vendored `annotate-snippets` crate, for the new diagnostics data model."
type: issue
state: closed
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
created_at: 2025-03-04T18:10:34Z
updated_at: 2025-03-14T18:59:35Z
url: https://github.com/astral-sh/ruff/issues/16506
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Build a renderer, on top of our vendored `annotate-snippets` crate, for the new diagnostics data model.

---

_Issue opened by @BurntSushi on 2025-03-04 18:10_

This issue is going to be a bit under-specified, but essentially, with the new `Diagnostic` type defined in #16503, we'll want to provide a routine for rendering it into a pleasing output. This is a prerequisite for actually using this type inside of Red Knot.

We already have an existing renderer build on top of the old diagnostic types. I expect to be able to re-use _some_ of that, but it was somewhat hamstrung by the more limited data model we were working with.

I believe the main challenge in building the renderer will be collecting annotations in a way that renders sensible. For example, multiple annotations might point to the same line of code. It will be the renderer's job to make sure we aren't showing the same line of code multiple times.

We will still use our vendored copy of `annotate-snippets`, so this shouldn't require doing the actual rendering logic for how to print code and annotations to a terminal.

---

_Referenced in [astral-sh/ruff#16504](../../astral-sh/ruff/issues/16504.md) on 2025-03-04 18:10_

---

_Renamed from "Build a renderer, on top of our vendored `annotate-snippets` crate, for the new data model." to "[red-knot] Build a renderer, on top of our vendored `annotate-snippets` crate, for the new diagnostics data model." by @BurntSushi on 2025-03-04 18:11_

---

_Label `red-knot` added by @BurntSushi on 2025-03-04 18:11_

---

_Label `diagnostics` added by @BurntSushi on 2025-03-04 18:11_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-03-04 18:15_

---

_Referenced in [astral-sh/ruff#16711](../../astral-sh/ruff/pulls/16711.md) on 2025-03-13 16:30_

---

_Closed by @BurntSushi on 2025-03-14 18:59_

---

_Closed by @BurntSushi on 2025-03-14 18:59_

---
